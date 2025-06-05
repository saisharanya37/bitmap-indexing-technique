# bitmap-indexing-technique
import java.util.Arrays;
import java.util.Scanner;
public class BitwiseOperations {
 // Convert input to a binary array
 public static int[] convertToBinaryArray(String input, int n) {
 // Try to interpret the input as a binary string
 try {
 // Ensure it's a valid binary string and then convert to an array of integers
 if (input.matches("[01]+")) {
 int[] binaryArray = new int[Math.min(input.length(), n)];
 for (int i = 0; i < binaryArray.length; i++) {
 binaryArray[i] = Character.getNumericValue(input.charAt(i));
 }
 return binaryArray;
 }
 } catch (Exception e) {
 // Continue to the next method if this fails
 }
 // Try to interpret the input as a string (convert to binary ASCII)
 try {
 byte[] bytes = input.getBytes();
 StringBuilder binaryString = new StringBuilder();
 for (byte b : bytes) {
 String binaryChar = String.format("%8s", Integer.toBinaryString(b & 0xFF)).replace(' ', '0');
 binaryString.append(binaryChar);
 }
 return convertToBinaryArray(binaryString.toString(), n);
 } catch (Exception e) {
 // Continue to the next method if this fails
 }
 // Try to interpret the input as a number
 try {
 int num = Integer.parseInt(input);
 String binaryString = Integer.toBinaryString(num);
 return convertToBinaryArray(binaryString, n);
 } catch (Exception e) {
 // Throw an error if none of the above methods work
 throw new IllegalArgumentException("Invalid input: Must be a binary string, a regular string, or a number.");
 }
 }
 public static int[] bitwiseOperations(int[] BMP1, int[] BMP2, int n, int W1) {
 // Ensure both arrays are of length n
 BMP1 = Arrays.copyOf(BMP1, n);
 BMP2 = Arrays.copyOf(BMP2, n);
 // Perform the AND operation
 int[] R = new int[n];
 for (int i = 0; i < n; i++) {
 R[i] = BMP1[i] & BMP2[i];
 }
 // Update BMP1 and BMP2 using XOR with R
 for (int i = 0; i < n; i++) {
 BMP1[i] = BMP1[i] ^ R[i];
 BMP2[i] = BMP2[i] ^ R[i];
 }
 // Calculate the sum of R and compare it to W1
 int sumR = 0;
 for (int value : R) {
 sumR += value;
 }
 return sumR > W1 ? R : new int[0];
 }
 public static void main(String[] args) {
 Scanner scanner = new Scanner(System.in);
 // Input size of BMP1
 System.out.println("Enter the size of BMP1: ");
 int sizeBMP1 = scanner.nextInt();
 // Input BMP1
 System.out.println("Enter BMP1 (binary string, regular string, or number): ");
 String bmp1Input = scanner.next();
 // Input size of BMP2
 System.out.println("Enter the size of BMP2: ");
 int sizeBMP2 = scanner.nextInt();
 // Input BMP2
 System.out.println("Enter BMP2 (binary string, regular string, or number): ");
 String bmp2Input = scanner.next();
 // Input n
 System.out.println("Enter the number of bits to process (n): ");
 int n = scanner.nextInt();
 // Input W1
 System.out.println("Enter the threshold value (W1): ");
 int W1 = scanner.nextInt();
 // Convert inputs to binary arrays
 int[] BMP1 = convertToBinaryArray(bmp1Input, sizeBMP1);
 int[] BMP2 = convertToBinaryArray(bmp2Input, sizeBMP2);
 // Run the bitwise operations
 try {
 int[] result = bitwiseOperations(BMP1, BMP2, n, W1);
 System.out.println("IB_Results: " + Arrays.toString(result));
 } catch (IllegalArgumentException e) {
 System.out.println("Error: " + e.getMessage());
 }
 scanner.close();
 }
}
