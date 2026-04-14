---
tags:
  - task
  - xpansiv
ticket: EMA-XXXX
status: ✅ Done
started: 2025-12-10
completed: Yes
---

# 🛠️ NEPOOL Registry Improve Performance of Sync Job

## Context
When there are transactions happening inside of NEPOOL Registry (SOAP Server) the sync job that was running periodically by Termpral was facing performance bottleneck due to parsing of XML SOAP responses from NEPOOL.

## Scope
Change of the parser from XPath to manual stream paser and unify the logic from 4 different areas that has been called.

## Technical Notes
- Feature flag: `NEPOOL_XML_PARSING_OPTIMIZE_REGISTRY_SYNC`
- Root cause: repeated XPath queries (e.g. `/ROOT/recLog/row[@TransferType='...']`) traversed the entire DOM on every call, plus DB lookups inside loops
- Fix: single-pass manual DOM traversal (`ApxRecParsedDocument`) + batch DB lookups upfront

### Performance results
| Run | Before | After |
|-----|--------|-------|
| 1 | 2m 48s | 2m 49s |
| 2 | 2m 33s | 1m 54s |
| 3 | 2m 34s | 1m 46s |

### Testing
Trigger sync manually via EvalGroovy:
```groovy
import com.apx.transact.refdata.RegistryProgramReference
import com.apx.util.spring.SpringAccess as $
$.registrySync.sync(RegistryProgramReference.NEPOOL);
```
- NEPOOL user EMAId: `098ACF87`
- Reporting period EMAId: `098A2A89`
- Audit logs: `/ema/runtime-data/audit-logs/`

#### Log to identify optimization running:
```Java
LOG.info("Beginning processing with " + (isUsingXMLOptimizationsEnabled? "optimized XML parsing" : "standard XML parsing"));
```

## Key Component Code

> [!INFO]- ApxRecParsedDocument class
> ```Java
> package com.apx.registry.services.impl;  
> import org.dom4j.Document;  
> import org.dom4j.Element;  
 >import org.dom4j.Node;  
 >import java.util.*; 
 >public class ApxRecParsedDocument {  
 >   final List<Element> issuanceRows;  
 >   final List<Element> automatedTransferRows;  
 >   final List<Element> pendingIncomingRows;  
 >   final List<Element> confirmRejectExpiryRows;  
 >   final Set<String> registryAccountIds;  
 >   ApxRecParsedDocument(  
 >       final List<Element> issuanceRows,  
 >       final List<Element> automatedTransferRows,  
 >       final List<Element> pendingIncomingRows,  
 >       final List<Element> confirmRejectExpiryRows,  
 >       final Set<String> registryAccountIds  
>    ) {  
 >       this.issuanceRows = issuanceRows;  
 >       this.automatedTransferRows = automatedTransferRows;  
 >       this.pendingIncomingRows = pendingIncomingRows;  
 >       this.confirmRejectExpiryRows = confirmRejectExpiryRows;  
 >       this.registryAccountIds = registryAccountIds;  
 >   }
 >   
 >   static ApxRecParsedDocument parseDocumentSinglePass(final Document document) {  
>  
 >       final List<Element> issuanceRows = new ArrayList<>();  
 >       final List<Element> automatedTransferRows = new ArrayList<>();  
 >       final List<Element> pendingIncomingRows = new ArrayList<>();  
 >       final List<Element> confirmRejectExpiryRows = new ArrayList<>();  
 >       final Set<String> registryAccountIds = new HashSet<>();  
>  
 >       final Element recLogElement = document.getRootElement().element("recLog");  
 >       if (recLogElement != null) {  
 >           for (int i = 0, size = recLogElement.nodeCount(); i < size; i++) {  
 >               final Node node = recLogElement.node(i);  
 >               if (node instanceof Element) {  
 >                   Element element = (Element) recLogElement.node(i);  
>  
 >                   final String fromAccountId = element.attributeValue("ahIDFrom");  
 >                   final String toAccountId = element.attributeValue("ahIDTo");  
 >                   if (fromAccountId != null) registryAccountIds.add(fromAccountId);  
 >                   if (toAccountId != null) registryAccountIds.add(toAccountId);  
 >                   final String action = element.attributeValue("rlAction");  
 >                   final String transferType = element.attributeValue("TransferType");  
 > 
 >                   if ("Certificate Creation".equals(transferType)) {  
 >                       issuanceRows.add(element);  
 >                   } else if ("Forward Transfer".equals(transferType) && "CT ".equals(action)) {  
 >                       automatedTransferRows.add(element);  
 >                   } else if (transferType == null) {  
 >                       if ("PEN".equals(action) || "RNW".equals(action) ||  
 >                           "WIT".equals(action) || "EXP".equals(action)) {  
 >                           pendingIncomingRows.add(element);  
 >                       }  
 > 
 >                       if ("CT ".equals(action) || "REJ".equals(action) || "EXP".equals(action)) {  
>                            confirmRejectExpiryRows.add(element);  
 >                       }  
 >                   }  
 >               }  
 >           }  
 >       } 
 >       return new ApxRecParsedDocument(  
 >           issuanceRows, automatedTransferRows, pendingIncomingRows,  
 >           confirmRejectExpiryRows, registryAccountIds);  
 >   }  
>}


>[!warn] Bottelneck
>```
>"/ROOT/recLog/row[@TransferType='Certificate Creation']"
>```
>Such XPath queries are extreamly slow while parsing bigger files


## Links
- https://github.com/dom4j/dom4j/wiki/Cookbook#fast-index-based-navigation
- [https://dom4j.github.io/javadoc/1.6.1/org/dom4j/Element.html#elementIterator()](https://dom4j.github.io/javadoc/1.6.1/org/dom4j/Element.html#elementIterator\(\) "https://dom4j.github.io/javadoc/1.6.1/org/dom4j/element.html#elementiterator()")

## Achievement (CV feed)
> One sentence: Implement stream read approach to speed up the XML parsing of SOAP responses by 10x in avg.
