---
layout: post
title:  OutputStream-InputStream와 Writer-Reader
date:   2021-09-11 00:00:00
categories: java
---

# 데이터 I/O

java에는 데이터 입력, 출력과 관련하여 다음과 같은 추상클래스가 정의되어 있습니다. 

#### 입력

- InputStream
  - 구현 클래스 : AudioInputStream, ByteArrayInputStream, FileInputStream, FilterInputStream, ObjectInputStream, PipedInputStream, SequenceInputStream, StringBufferInputStream
- Reader
  - 구현 클래스 : BufferedReader, CharArrayReader, FilterReader, InputStreamReader, PipedReader, StringReader, URLReader

#### 출력

- OutputStream
  - 구현 클래스 : ByteArrayOutputStream, FileOutputStream, FilterOutputStream, ObjectOutputStream, PipedOutputStream
- Writer
  - 구현 클래스 : BufferedWriter, CharArrayWriter, FilterWriter, OutputStreamWriter, PipedWriter, PrintWriter, StringWriter



#### InputStream - Reader / OutputStream - Writer의 근본적 차이는 데이터를 어떻게 다루냐에 있습니다.

#### InputStream - OutputStream

- 데이터를 **byte 단위**로 입력하고 출력
- 바이너리 형태 데이터를 다룰 때 사용



#### Reader - Writer

- 데이터를 **char 문자 단위**로 입력하고 출력
- 바이트 코드를 읽어서 문자로 변환하고 문자를 바이트 코드로 변환해서 출력하는 것을 처리해줌
  - 기본 인코딩 형식은 JVM의 default charset을 사용 (내부적으로 Charset.defaultCharset() 호출)
  - 생성자 파라미터로 특정 인코딩 형식을 전달하여 생성할 수도 있음
- 일반적인 텍스트 파일을 읽을때는 Reader와 Writer의 구현 클래스를 사용



> [주의점]
>
> 실무에서 파일이나 소켓 통신으로 타 기관과 데이터를 송수신하여 처리하는 경우에, 데이터 레이아웃을 정의할때 각 필드 길이가 문자 단위인지 byte 단위인지 명확히 해야합니다.
>
> 예를 들어, 정보 제공처에서는 "주소"라는 필드에 50byte를 할당하고, 수신처에서는 "주소" 필드를 길이 50의 문자열로 자르게 된다면 전혀 다른 형태로 데이터 변환이 일어나게 됩니다.



#### 예제 - 파일을 이용한 데이터 입출력 테스트

OutputStream과 Writer의 구현 클래스로 한줄씩 파일에 출력하고, InputStream과 Read로 파일 읽기

```java
public void fileOutputStreamTest(String filename){
  String data="this is output Stream test";
  byte[] bytes = data.getBytes(); // getBytes()를 호출하면 내부적으로 String을 바이너리 형식으로 인코딩함
  try (OutputStream out = new FileOutputStream(filename)) {
    out.write(bytes);
  } catch (FileNotFoundException e) {
    e.printStackTrace();
  } catch (IOException e) {
    e.printStackTrace();
  }
}

public void fileWriterTest(String filename){
  String data="this is writer test";
  try(Writer fileWriter=new FileWriter(filename,true)){
    try(BufferedWriter bufferedWriter = new BufferedWriter(fileWriter)){
      bufferedWriter.newLine();
      bufferedWriter.write(data);
    }
  }catch (FileNotFoundException e) {
    e.printStackTrace();
  } catch (IOException e) {
    e.printStackTrace();
  }
}

public void fileInputStreamTest(String filename){

  try(InputStream in = new FileInputStream(filename)) {
    byte[] bytes = in.readAllBytes(); // 바이너리 데이터를 그대로 읽어서
    String data = new String(bytes,Charset.defaultCharset()); // 두번째 파라미터로 인코딩 형식을 지정할 수 있음
    System.out.println("inputstream : "+data);
  } catch (FileNotFoundException e) {
    e.printStackTrace();
  } catch (IOException e) {
    e.printStackTrace();
  }
}

public void fileReadTest(String filename){
  try (Reader fileReader=new FileReader(filename)){
    try(BufferedReader br = new BufferedReader(fileReader)){
      String line=null;
      while((line=br.readLine())!=null){
        System.out.println("Reader : "+line);
      }
    }
  } catch (FileNotFoundException e) {
    e.printStackTrace();
  } catch (IOException e) {
    e.printStackTrace();
  }
}
```

> [output]
>
> inputstream : this is output Stream test
> this is writer test
> Reader : this is output Stream test
> Reader : this is writer test



[참고] [oracle docs 11](https://docs.oracle.com/en/java/javase/11/docs/api/allclasses.html)
