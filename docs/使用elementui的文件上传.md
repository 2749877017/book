# 使用elementui的文件上传

## 一、前端

### 1、template模板

```html
<template>
    <div>
        <el-button type="info" @click="subupload">文件上传</el-button>
        <el-upload class="upload-demo" 
        drag 
        action="api/zsgc/zsgc/do/upload" 
        :auto-upload="autoupload" 
        multiple 
        ref="upload">
            <i class="el-icon-upload"></i>
            <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
            <div class="el-upload__tip" slot="tip">只能上传jpg/png文件，且不超过500kb</div>
        </el-upload>


    </div>

</template>
```

### 2、逻辑代码

```js
<script>
    export default {
        data() {
            return {
                autoupload: false




            }

        },
        methods: {
            subupload() {
                this.$refs.upload.submit();
            }





        }




    }
</script>
```

## 二、后端

```java
@ResponseBody
    @RequestMapping("/upload")
    public String upload(HttpServletRequest request) throws IOException {
        MultipartHttpServletRequest mrequest=(MultipartHttpServletRequest)request;
        Iterator<String> files=mrequest.getFileNames();
        while (files.hasNext()){
        MultipartFile mfile=mrequest.getFile(files.next());
        String oldfilename=mfile.getOriginalFilename();
        String newfilename= UUID.randomUUID().toString()+oldfilename.substring(oldfilename.lastIndexOf("."),oldfilename.length());
        mfile.transferTo(new File("C:\\Users\\liang\\Desktop\\a",newfilename));
            System.out.println("上传成功");
        }

        return "success";

    }
```
