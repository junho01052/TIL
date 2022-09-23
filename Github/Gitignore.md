# Gitignore
## .gitignore
의도적으로 파일을 무시하도록 지정합니다.
*Git에서 이미 추적한 파일은 영향을 받지 않습니다.  

## 이미 push된 파일을 제거하는 법
### 원격저장소, 로컬저장소에서 모두 제거하기
```bash
$ git rm 파일명
$ git rm -r 폴더명
```
### 원격저장소에서만 제거하기
```bash
$ git rm --cached 파일명
$ git rm --cached -r 폴더명
```