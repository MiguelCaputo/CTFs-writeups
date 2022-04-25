# I Love Java
We are given a java class that we can decompile with: http://www.javadecompilers.com/

The decompile code is: 

```
import java.util.Random;
import java.util.Scanner;

public class CrackMe
{
    public static void main(final String[] array) {
        final Scanner scanner = new Scanner(System.in);
        System.out.println("What is the flag?");
        final String nextLine = scanner.nextLine();
        if (nextLine.length() != 22) {
            System.out.println("Not the flag :(");
            return;
        }
        final char[] array2 = new char[nextLine.length()];
        for (int i = 0; i < nextLine.length(); ++i) {
            array2[i] = nextLine.charAt(i);
        }
        for (int j = 0; j < nextLine.length() / 2; ++j) {
            final char c = array2[nextLine.length() - j - 1];
            array2[nextLine.length() - j - 1] = array2[j];
            array2[j] = c;
        }
        final int[] array3 = { 19, 17, 15, 6, 9, 4, 18, 8, 16, 13, 21, 11, 7, 0, 12, 3, 5, 2, 20, 14, 10, 1 };
        final int[] array4 = new int[array2.length];
        for (int k = array3.length - 1; k >= 0; --k) {
            array4[k] = array2[array3[k]];
        }
        final Random random = new Random();
        random.setSeed(431289L);
        final int[] array5 = new int[nextLine.length()];
        for (int l = 0; l < nextLine.length(); ++l) {
            array5[l] = (array4[l] ^ random.nextInt(l + 1));
        }
        String s = "";
        for (int n = 0; n < array5.length; ++n) {
            s += array5[n]);
        }
        System.out.println(s);
        if (s.equals("116.122.54.50.93.66.98.117.75.51.97.78.104.119.90.53.94.36.105.84.40.69.")) {
            System.out.println("Congrats! You got the flag!");
        }
        else {
            System.out.println("Not the flag :(");
        }
    }
}
```

We need to pass a 22 character flag and find the answer, to solved it I made some changes in the code to see what the value were, first I modified the XOR of array 4 like:

```
for (int l = 0; l < nextLine.length(); ++l) {
	array5[l] = (array5[l] ^ random.nextInt(l + 1));
	System.out.println(array5[l] + " ");
}
```

This gave me: 
>t{43_GftJ4bIh}_0_4cV$T 

Which looks like a flag, now we now it is shuffled a little bit because of this part of the code:

```
final int[] array3 = { 19, 17, 15, 6, 9, 4, 18, 8, 16, 13, 21, 11, 7, 0, 12, 3, 5, 2, 20, 14, 10, 1 };
final int[] array4 = new int[array2.length];
for (int k = array3.length - 1; k >= 0; --k) {
	array4[k] = array2[array3[k]];
}
```

We can change array 3 to:

> 13, 21, 17, 15, 5, 16, 3, 12, 7, 4, 20, 11, 14, 9, 19, 2, 8, 1, 6, 0, 18, 10

When we run it now passing "t{43_GftJ4bIh}_0_4cV$T" as the flag we get:

>}T40G_3ht_$I_4V4J{ftcb

Lastly we now that the flag is reversed, after we reverse it back we get the final flag

## Flag

> bctf{J4V4_I$_th3_G04T}




