# Code a method that list all permutations that can make with letters in a given string

```Java
import java.lang.*;
import java.util.*;
import java.util.stream.*;
import java.io.*;

public class StringPermutations {
    
    public static List<String> permutation(String input) {
        // your solution goes herez
        return List.of();
    }
    
    private static void test_singleWord(List<String> actual) {
        List<String> expected = Collections.singletonList("1");
        System.out.println("Single word Test: " +
        (listEqualsIgnoreOrder(expected, actual) ? "PASSED" : "FAILED"));
    }
    
    private static void test_ABC(List<String> actual) {
        List<String> expected = Arrays.asList("ABC", "ACB", "BAC", "BCA", 
        "CAB", "CBA");
        System.out.println("ABC Test: " + 
        (listEqualsIgnoreOrder(expected, actual) ? "PASSED" : "FAILED"));
    }
    
    private static void test_1234(List<String> actual) {
        List<String> expected = Arrays.asList("1234", "1243", "1324", "1342",
        "1423", "1432", "2134", "2143", 
        "2314", "2341", "2413", "2431", "3124", "3142", "3214", "3241", "3412",
        "3421", "4123", "4132", "4213", "4231", "4312", "4321");
        System.out.println("1234 Test: " + 
        (listEqualsIgnoreOrder(expected, actual) ? "PASSED" : "FAILED"));
    }
    
    public static boolean listEqualsIgnoreOrder(
	    List<String> list1, 
	    List<String> list2
	) {
    return new HashSet<>(list1).equals(new HashSet<>(list2));
}
    
    public static void main(String args[]) {
      test_singleWord(permutation("1"));
      test_ABC(permutation("ABC"));
      test_1234(permutation("1234"));
      
    }
}
```