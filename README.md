# jarpic-client
A Simple JSON-RPC 2.0 Java Client using Jackson and OkHttp

This library works on Android as well

It contains both synchronous and asynchronous APIs.

[![Build Status](https://travis-ci.org/resourcepool/jarpic-client.svg?branch=master)](https://travis-ci.org/resourcepool/jarpic-client)

## Add it to your project
Maven:
```xml
<!-- https://mvnrepository.com/artifact/io.resourcepool/jarpic-client -->
<dependency>
    <groupId>io.resourcepool</groupId>
    <artifactId>jarpic-client</artifactId>
    <version>1.1.0</version>
</dependency>
```
Gradle:
```groovy
compile 'io.resourcepool:jarpic-client:1.1.0'
```

## Usage

Send single JSON-RPC request:

```java
JsonRpcClient client = new HttpJsonRpcClient(endpoint);
JsonRpcRequest req = JsonRpcRequest.builder()
  .method("cmd::execCmd")
  .param("param1", "myvalue1")
  .param("param2", "myvalue2")
  .build();
  
// With your own Result class POJO  
JsonRpcResponse<Result> res = client.send(req, Result.class);
System.out.println("Response is:");
System.out.println(res);
```

Send multiple JSON-RPC requests:

```java
JsonRpcClient client = new HttpJsonRpcClient(endpoint);
JsonRpcRequest req1 = JsonRpcRequest.builder()
  .method("cmd::execCmd")
  .param("param1", "myvalue1")
  .param("param2", "myvalue2")
  .build();

JsonRpcRequest req2 = JsonRpcRequest.builder()
  .method("cmd::resumeCmd")
  .param("key", "value")
  .build();
  
List<JsonRpcRequest> reqs = JsonRpcRequest.combine(req1, req2);
  
// With your own Result class POJO  
List<JsonRpcResponse<Result>> res = client.send(reqs, Result.class);
System.out.println("Response is:");
System.out.println(res);
```

Send single JSON-RPC Notification
```java
JsonRpcClient client = new HttpJsonRpcClient(endpoint);
JsonRpcRequest req = JsonRpcRequest.notifBuilder()
  .method("cmd::execCmd")
  .param("param1", "myvalue1")
  .param("param2", "myvalue2")
  .build();
  
// With your own Result class POJO  
JsonRpcResponse<Result> res = client.send(req, Result.class);
System.out.println("Response is:");
System.out.println(res);
```

**All these methods can also be called asynchronously by providing an extra parameter.**
  
Simple Example:
```java
JsonRpcClient client = new HttpJsonRpcClient(endpoint);
JsonRpcRequest req = JsonRpcRequest.builder()
  .method("cmd::execCmd")
  .param("param1", "myvalue1")
  .param("param2", "myvalue2")
  .build();
  
client.send(req, String.class, new JsonRpcCallback() {
      @Override
      public void onResponse(@Nullable JsonRpcResponse res) {
        System.out.println("Response is:");
        System.out.println(res);
      }

      @Override
      public void onFailure(IOException ex) {
        System.err.println("Something bad happened: " + ex.getMessage());
      }
    });

```

Example with custom deserializer:
```java
JsonRpcRequest req = JsonRpcRequest.builder()
      .method("cmd::start")
      .param("apiKey", apiKey)
      .build();

client.send(req, Result.class, new JsonRpcCallback<Result>() {
      @Override
      public void onResponse(@Nullable JsonRpcResponse<Result> res) {
        // With your own Result class POJO  
        System.out.println(res.getResult());
      }

      @Override
      public void onFailure(IOException ex) {
        System.err.println("Something bad happened: " + ex.getMessage());
      }
    });
```

## License
   Copyright 2017 Resourcepool

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
