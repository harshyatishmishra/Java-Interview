
#### First repeating character from the String.

```
  import java.util.HashSet;
  import java.util.Set;

  public class FirstRepeatedChar {

    public static void main(String[] args) {

      String input = "God";

      System.out.println(firstRepeatedCharacter(input));

    }
```
```
    private static char firstRepeatedCharacter(String input) {
      char[] charArray = input.toCharArray();
      Set<Character> unique = new HashSet<Character>();

      for (int i = 0; i < charArray.length; i++) {
        if (unique.contains(charArray[i])) {
          return charArray[i];
        } else {
          unique.add(charArray[i]);
        }
      }
      return '0';
    }

  }
```
