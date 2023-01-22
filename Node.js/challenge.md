## 요구사항 정리

- 사진 폴더 정리
  - png | aae → captured
  - mp4 | mov → video
  - IMG_E1234 & IMG_1234일 경우 원본인 IMG_1234를 duplicated로 이동

```jsx
const fs = require('fs').promises;
const path = require('path');
const process = require('process');

const targetDir = process.argv[2];

if (targetDir === undefined) {
  console.error('Define target directory');
  return;
}

const dirs = {
  captured: path.join(__dirname, targetDir, 'captured'),
  duplicated: path.join(__dirname, targetDir, 'duplicated'),
  video: path.join(__dirname, targetDir, 'video'),
};

for (const [key, value] of Object.entries(dirs)) {
  fs.access(value).catch(() => fs.mkdir(value));
}

fs.readdir(path.join(__dirname, targetDir))
  .then((files) => {
    files.forEach((file) => {
      switch (path.extname(file)) {
        case '.mp4':
        case '.mov':
          // move to video directory
          fs.rename(
            path.join(__dirname, targetDir, file),
            path.join(__dirname, targetDir, 'video', file)
          ).catch(console.error);
          break;
        case '.png':
        case '.aae':
          // move to captured directory
          fs.rename(
            path.join(__dirname, targetDir, file),
            path.join(__dirname, targetDir, 'captured', file)
          ).catch(console.error);
          break;
        case '.jpg':
        case '.jpeg':
          // check dup
          // skip edited file
          if (file.startsWith('IMG_E')) {
            break;
          }
          console.log(file);
          fs.access(
            path.join(__dirname, targetDir, 'IMG_E' + file.substring(4))
          )
            .then(() => {
              fs.rename(
                path.join(__dirname, targetDir, file),
                path.join(__dirname, targetDir, 'duplicated', file)
              ).catch(console.error);
            })
            .catch(console.error);
          break;
        default:
          break;
      }
    });
  })
  .catch(console.error);
```
