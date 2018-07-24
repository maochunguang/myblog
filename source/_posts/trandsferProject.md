title: 用java将GBK工程转为uft8
date: 2017-08-12 07:43:59
tags: java
categories: 编程语言
---
** {{ title }}：** <Excerpt in index | 首页摘要>
windows下的默认编码为GBK还有gb2312，如何把gbk的java工程转为utf8的呢，如果直接修改工程编码，其实里面的java文件中中文是会乱码的，写了个批量转换java工程的程序，消遣一下。
<!-- more -->
<The rest of contents | 余下全文>
## 为什么要转码？
有些老的项目，或者朋友的项目之前没注意在windows上不是utf8，而你有需要看注释或者什么，总不能一个文件一个文件的去改编码属性吧。

## 本程序试用范围
gbk的代码，或者gb2312的工程均可以转换

## 编码转换的思路
本来想做成一个通用的会自动检测编码，自动转换的程序。但是由于判断编码类型不准，所以做成了针对GBK的转换。
1. 制定gbk编码把文件流读进来，加载到内存，转为String类型的内容
2. 将String内容转为utf8的String
3. 将String内容写入文件

## 核心代码：

```java
public class TransferProject {
    public static void transferFile(String pathName, int depth) throws Exception {
        File dirFile = new File(pathName);
        if (!isValidFile(dirFile)) return;
        //获取此目录下的所有文件名与目录名
        String[] fileList = dirFile.list();
        int currentDepth = depth + 1;
        for (int i = 0; i < fileList.length; i++) {
            String string = fileList[i];
            File file = new File(dirFile.getPath(), string);
            String name = file.getName();
            //如果是一个目录，搜索深度depth++，输出目录名后，进行递归
            if (file.isDirectory()) {
                //递归
                transferFile(file.getCanonicalPath(), currentDepth);
            } else {
                if (name.contains(".java") || name.contains(".properties") || name.contains(".xml")) {
                    readAndWrite(file);
                    System.out.println(name + " has converted to utf8 ");
                }
            }
        }
    }

    private static boolean isValidFile(File dirFile) throws IOException {
        if (dirFile.exists()) {
            System.out.println("file exist");
            return true;
        }
        if (dirFile.isDirectory()) {
            if (dirFile.isFile()) {
                System.out.println(dirFile.getCanonicalFile());
            }
            return true;
        }
        return false;
    }

    private static void readAndWrite(File file) throws Exception {
        String  content = FileUtils.readFileByEncode(file.getPath(), "GBK");
        FileUtils.writeByBufferedReader(file.getPath(), new String(content.getBytes("UTF-8"), "UTF-8"));
    }

    public static void main(String[] args) throws Exception {
        //程序入口，制定src的path
        String path = "/Users/mac/Downloads/unit06_jdbc/src";
        transferFile(path, 1);
    }
}
```

```java
public class FileUtils {
    public static void writeByBufferedReader(String path, String content) {
        try {
            File file = new File(path);
            file.delete();
            if (!file.exists()) {
                file.createNewFile();
            }

            FileWriter fw = new FileWriter(file, false);
            BufferedWriter bw = new BufferedWriter(fw);
            bw.write(content);
            bw.flush();
            bw.close();

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    public static String readFileByEncode(String path, String chatSet) throws Exception {
        InputStream input = new FileInputStream(path);
        InputStreamReader in = new InputStreamReader(input, chatSet);
        BufferedReader reader = new BufferedReader(in);
        StringBuffer sb = new StringBuffer();
        String line = reader.readLine();
        while (line != null) {
            sb.append(line);
            sb.append("\r\n");
            line = reader.readLine();
        }
        return sb.toString();
    }
}
```
## 总结
遇到类似的问题，都可以试着用代码来进行实现，给自己的编码带来一些新的乐趣，也增加自己的信心。







> 博客搬家，请访问新博客地址吧! [我的博客][1]

[1]: https://www.duduhuahua.cn
