# -_Java
тестовое задание Калькулятор_Java
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String input = scanner.nextLine();
        scanner.close();
        System.out.println(calc(input));
    }

    public static String calc(String input) {
        String[] parts = input.split(" ");
        if (parts.length != 3) {
            throw new IllegalArgumentException("Неправильный формат ввода");
        }

        String operand1 = parts[0];
        String operator = parts[1];
        String operand2 = parts[2];

        int num1;
        int num2;
        boolean isRoman = false;

        try {
            num1 = Integer.parseInt(operand1);
            num2 = Integer.parseInt(operand2);
        } catch (NumberFormatException e) {
            num1 = RomanToArabic(operand1);
            num2 = RomanToArabic(operand2);
            isRoman = true;
        }

        if (num1 < 1 || num1 > 10 || num2 < 1 || num2 > 10) {
            throw new IllegalArgumentException("Недопустимые числа");
        }

        int result;
        switch (operator) {
            case "+":
                result = num1 + num2;
                break;
            case "-":
                result = num1 - num2;
                break;
            case "*":
                result = num1 * num2;
                break;
            case "/":
                result = num1 / num2;
                break;
            default:
                throw new IllegalArgumentException("Неподдерживаемая операция");
        }

        return isRoman ? ArabicToRoman(result) : String.valueOf(result);
    }

    private static int RomanToArabic(String roman) {
        Map<Character, Integer> romanToArabic = new HashMap<>();
        romanToArabic.put('I', 1);
        romanToArabic.put('V', 5);
        romanToArabic.put('X', 10);
        romanToArabic.put('L', 50);
        romanToArabic.put('C', 100);
        romanToArabic.put('D', 500);
        romanToArabic.put('M', 1000);

        int result = 0;
        int prevValue = 0;
        for (int i = roman.length() - 1; i >= 0; i--) {
            int value = romanToArabic.get(roman.charAt(i));
            if (value < prevValue) {
                result -= value;
            } else {
                result += value;
                prevValue = value;
            }
        }

        return result;
    }

    private static String ArabicToRoman(int number) {
        if (number < 1) {
            throw new IllegalArgumentException("Недопустимое число");
        }

        StringBuilder result = new StringBuilder();
        int[] arabicToRoman = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        String[] romanSymbols = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

        int i = 0;
        while (number > 0) {
            if (number >= arabicToRoman[i]) {
                result.append(romanSymbols[i]);
                number -= arabicToRoman[i];
            } else {
                i++;
            }
        }

        return result.toString();
    }
}
