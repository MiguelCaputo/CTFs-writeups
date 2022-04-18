# Covfefe

We are given a java class that we can decompile with: http://www.javadecompilers.com/

The decompiled code is 

```
public class Covfefe

    public static void main(final String[] array) {
        final int n = 35;
        final int[] array2 = new int[n];
        for (int i = 0; i < n; ++i) {
            array2[i] = 0;
        }
        array2[0] = 103;
        array2[1] = array2[0] + 2;
        array2[2] = array2[0];
        for (int j = 3; j < 8; ++j) {
            switch (j) {
                case 3: {
                    array2[j] = 101;
                    break;
                }
                case 4: {
                    array2[6] = 99;
                    break;
                }
                case 5: {
                    array2[5] = 123;
                    break;
                }
                case 6: {
                    array2[j + 1] = 48;
                    break;
                }
                case 7: {
                    array2[4] = 109;
                    break;
                }
            }
        }
        array2[9] = (array2[8] = 10);
        final int[] array3 = array2;
        final int n2 = 24;
        final int[] array4 = array2;
        final int n3 = 25;
        final int[] array5 = array2;
        final int n4 = 28;
        final int n5 = array2[7];
        array5[n4] = n5;
        array3[n2] = (array4[n3] = n5);
        array2[10] = 51;
        array2[11] = array2[10] + 12 - 4 - 4 - 4;
        final int[] array6 = array2;
        final int n6 = 12;
        final int[] array7 = array2;
        final int n7 = 15;
        final int[] array8 = array2;
        final int n8 = 22;
        final int[] array9 = array2;
        final int n9 = 27;
        final int n10 = array2[0] - (int)Math.pow(2.0, 3.0);
        array8[n8] = (array9[n9] = n10);
        array6[n6] = (array7[n7] = n10);
        array2[13] = 49;
        array2[14] = 115;
        for (int k = 16; k < 22; ++k) {
            switch (k) {
                case 16: {
                    array2[k + 1] = 108;
                    break;
                }
                case 17: {
                    array2[k - 1] = 52;
                    break;
                }
                case 18: {
                    array2[k + 1] = 52;
                    break;
                }
                case 19: {
                    array2[k - 1] = 119;
                    break;
                }
                case 20: {
                    array2[k + 1] = 115;
                    break;
                }
                case 21: {
                    array2[k - 1] = 121;
                    break;
                }
            }
        }
        array2[23] = 103;
        array2[26] = array2[23] - 3;
        array2[29] = array2[26] + 20;
        array2[30] = array2[29] % 53 + 53;
        array2[31] = array2[0] - 18;
        array2[32] = 80;
        array2[33] = 83;
        array2[n - 1] = (int)Math.pow(5.0, 3.0);
    }
}
```

There is some array manipulation happening, if we add:

```
for (int i = 0; i < n; ++i) {
	System.out.println(array2[i]);
}
```

After the last line of code we can print the flag as integers, we can compile the code like: 

```
javac Covfefe.java
```

and then run it like:

```
java Covfefe
```

which will print integers stored in the array but we can convert them to characters and get the flag

# Flag
> gigem{c0ff33_1s_4lw4ys_g00d_0xCUPS}

