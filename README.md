# Telephone-number
package com.javarush.task.task22.task2212;
import java.io.BufferedReader;
import java.io.FileReader;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
/* 
Проверка номера телефона
*/
public class Solution {
    public static void main(String[] args) {
		System.out.println(checkTelNumber("+380501234567")); //true
		System.out.println(checkTelNumber("+38(050)1234567")); //true
		System.out.println(checkTelNumber("+38(050)123-45-67")); //true
		System.out.println(checkTelNumber("(050)1234567")); //true
		System.out.println(checkTelNumber("0501234567")); //true
		System.out.println(checkTelNumber("+38050123-45-67")); //true
		System.out.println(checkTelNumber("050123-4567")); //true

		System.out.println();
		System.out.println(checkTelNumber("38)050(1234567")); //false
		System.out.println(checkTelNumber("+38(050)1-23-45-6-7")); //false
		System.out.println(checkTelNumber("050ххх4567")); //false
		System.out.println(checkTelNumber("050123456")); //false
		System.out.println(checkTelNumber("+01234567891234")); //false
		System.out.println(checkTelNumber("(0)501234567")); //false
	}

	public static boolean checkTelNumber(String telNumber) {
		if (telNumber == null || telNumber.isEmpty()) return false;
		int length = telNumber.length();
		int countDigits = telNumber.replaceAll("\\D", "").length();
		int indOpen = telNumber.indexOf("(");
		int indClose = telNumber.indexOf(")");
		int indMinus = telNumber.indexOf("-");
		if ((telNumber.charAt(0) == '+') && countDigits != 12) return false;
		if (telNumber.matches("(^\\(|^\\d)\\S+") && countDigits != 10) return false;
		if ((length - telNumber.replaceAll("\\-", "").length()) > 2) return false;
		if ((length - telNumber.replaceAll("\\-{2}", "").length()) > 0) return false;
		if (indOpen != -1 && (length - telNumber.replaceAll("\\(", "").length()) != 1) return false;
		if (indClose != -1 && (length - telNumber.replaceAll("\\)", "").length()) != 1) return false;
		if (indOpen != -1 && indClose != -1 && indClose - indOpen != 4) return false;
		if (indClose != -1 && indMinus != -1 && indMinus < indClose) return false;
		if (!telNumber.matches("\\S+\\d$")) return false;
		try {
			Long.parseLong(telNumber.replaceAll("[\\(,\\),\\+\\-]", ""));
		} catch (Exception e) {
			return false;
		}
		return true;
	}
}
