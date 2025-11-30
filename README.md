# Working-SMSService

Java SMS Service Module

A simple and reusable Java-based SMS sending module that can be integrated into any existing project. This service works as a plug-and-play component. Just configure your SMS gateway credentials and call the service to send SMS from your application.

Features

Lightweight Java module for sending SMS

Works with any SMS gateway API

Easy integration with Spring Boot, Java EE, or plain Java projects

Centralized SMS utility class

Supports templates and dynamic message text

Highly configurable service layer

Production-ready structure

How It Works

This module sends SMS using your preferred SMS API provider.
You only need to update the API URL, API Key, Sender ID, and other required parameters in the configuration section.



public class SmsConfig {
    public static final String API_URL = "https://your-sms-api-url";
    public static final String API_KEY = "YOUR_API_KEY";
    public static final String SENDER_ID = "SENDERID";
}

import java.net.HttpURLConnection;
import java.net.URL;
import java.net.URLEncoder;

public class SmsClient {

    public static String send(String phone, String message) {
        try {
            String charset = "UTF-8";
            String query = String.format("apikey=%s&sender=%s&number=%s&message=%s",
                    URLEncoder.encode(SmsConfig.API_KEY, charset),
                    URLEncoder.encode(SmsConfig.SENDER_ID, charset),
                    URLEncoder.encode(phone, charset),
                    URLEncoder.encode(message, charset));

            URL url = new URL(SmsConfig.API_URL + "?" + query);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("GET");

            return "Response Code: " + conn.getResponseCode();
        } catch (Exception e) {
            return "Error: " + e.getMessage();
        }
    }
}


public class SmsService {

    public static String sendSms(String phone, String content) {
        return SmsClient.send(phone, content);
    }
}
