# Cloudinary

- 사용자의 이미지를 업로드하거나 Resizing, Transforming, Filtering 할수 있는 서비스
- 비디오, 파일, PDF 등은 Firebase의 Cloud Storage를 이용해서 업로드 가능
- Firebase대신 Cloudinary를 사용하는 이유는, 업로드 한 사진을 다양한 방식으로 변경이 가능하기 때문임 (Optimizing)
- 이 모든것들을 `무료` 로 이용할 수 있는 플랜이 존재함

[Image and Video Upload, Storage, Optimization and CDN](https://cloudinary.com/)

### Cloudinary에서 제공하는 서비스

- Image APIs
- Video APIs
- Digital Asset Management
- Integrations

### 프로젝트 준비용 설정 내용

```bash
1. Cloudinary 가입
2. Settings > new upload template > unsigned template 작성
3. Incoming Tranformation에서 Scale 500x500 적용
```

### Document 확인 하는 방법

[Cloudinary Media Upload: Upload Images, Videos & Other Files | Cloudinary](https://cloudinary.com/documentation/image_video_and_file_upload)

- APIs를 이용해서 업로드 구현 (REST APIs)
- Cloudinary SDKs
