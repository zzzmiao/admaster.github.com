---
weight: 1
layout: default
category: trackmaster
subcategory: sdk
language: cn
title: Java SDK
---

# TrackMaster API Java SDK

### 示例类(client.java)

{:.prettyprint}
    import java.io.*;
    import java.net.*;

    public class client{
        URL url;
        String urlStr;
        HttpURLConnection conn;
        BufferedReader reader;
        String line;
        String result = "";

        public client(String urlToRead){
            urlStr = urlToRead;
        }

        public String get(String param) {

            try {
                url = new URL(urlStr + param);
                conn = (HttpURLConnection) url.openConnection();
                conn.setRequestMethod("GET");
                conn.setRequestProperty("User-Agent", "TrackMaster-SDK-JAVA");
                reader = new BufferedReader(new InputStreamReader(conn.getInputStream()));
                while ((line = reader.readLine()) != null) {
                    result += line;
                }
                reader.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
            return result;
       }

        public String post(String param, String json) {

            try {
                url = new URL(urlStr + param);
                conn = (HttpURLConnection) url.openConnection();
                conn.setDoOutput(true);
                conn.setDoInput(true);
                conn.setInstanceFollowRedirects(false);
                conn.setRequestMethod("POST");
                conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
                conn.setRequestProperty("User-Agent", "TrackMaster-SDK-JAVA");
                conn.setRequestProperty("charset", "utf-8");
                conn.setRequestProperty("Content-Length", "" + Integer.toString(json.getBytes().length));
                conn.setUseCaches (false);
                OutputStreamWriter writer = new OutputStreamWriter(conn.getOutputStream());
                writer.write(json);
                writer.flush();
                reader = new BufferedReader(new InputStreamReader(conn.getInputStream()));
                while ((line = reader.readLine()) != null) {
                    result += line;
                }
                writer.close();
                reader.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
            return result;
        }
    }


### 获取当前用户/获取当前用户网络/获取access_token(myMain.java)

{:.prettyprint}
    class myMain{
        public static void main(String args[])
       {
           client apiClient = new client("http://track.admasterapi.com");
           String access_token = "您的access token";
           System.out.println(apiClient.get("/user?access_token=" + access_token)); // 获取用户信息
           System.out.println(apiClient.get("/user/networks?access_token=" + access_token)); // 获取用户网络信息

           // 构造json，格式为{"client_id":"aaa","client_secrect":"bbb","grant_type": "ccc","email":"ddd","password":"eee"}
           String client_id = "\"您的client_id\"";
           String client_secret = "\"您的client_secrect\"";
           String grant_type = "\"password\"";
           String password = "\"您的密码\"";
           String email = "\"您的邮箱\"";
           String json = "{\"client_id\":" + client_id
               + ",\"client_secret\":" + client_secret
               + ",\"grant_type\":" + grant_type
               + ",\"email\":" +email
               + ",\"password\":" +password+"}";

           client openClient = new client("http://open.admaster.com.cn");

           System.out.println(openClient.post("/oauth/access_token", json)); // 获取access_token
       }
    }
