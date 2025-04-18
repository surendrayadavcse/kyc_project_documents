

## 📂 Image/Document Storage & Access in KYC System

### 📁 How We Store Uploaded Images/Documents

- When users upload KYC documents (like Aadhar, PAN, etc.), they are stored in a local folder named `uploadeddoc` in the root directory of the project.
- The absolute path to this folder is dynamically constructed using:
  ```java
  System.getProperty("user.dir") + File.separator + "uploadeddoc"
  ```
  This ensures that the storage location is relative to the server's running directory, making it OS-independent.

### 🌐 How We Serve/Access Uploaded Files

- We expose the uploaded files via HTTP so that they can be accessed directly from the frontend or browser.
- This is configured in the `WebConfig` class by implementing `WebMvcConfigurer`.

```java
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    String uploadPath = Paths.get(UPLOAD_DIR).toUri().toString();
    registry.addResourceHandler("/uploadeddoc/**")
            .addResourceLocations(uploadPath);
}
```

### 🔗 Example

If a file is saved as:
```
/project-root/uploadeddoc/selfie123.png
```

It can be accessed through:
```
http://localhost:8080/uploadeddoc/selfie123.png
```

This makes it easy for the admin to preview documents or for users to review their uploaded files.

---

