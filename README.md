# react-with-django
react build + django backend

### [참조한 Github](https://github.com/kieronjmckenna/react-in-django-static-files)

### [참조한 블로그](https://medium.com/codex/deploying-react-through-djangos-static-files-part-1-dev-setup-8a3a7b93c809)


### 1. 필수 설치 사항
```bash
django-admin startproject backend && npx create-react-app frontend
cd backend && pip install django whitenoise
```

### 2. 수정사항
```python
#settings.py
MIDDLEWARE = [
  "whitenoise.middleware.WhiteNoiseMiddleware",
  
  
TEMPLATES = [
  {
    "BACKEND": "django.template.backends.django.DjangoTemplates",
    # Tell Django where to find Reacts index.html file
    "DIRS": [os.path.join(BASE_DIR, "build")],

# React 의 빌드 결과를 바라보기 위한 path
STATICFILES_DIRS = [
  # Tell Django where to look for React's static files (css, js)
  os.path.join(BASE_DIR, "build/static"),
]

STATICFILES_STORAGE = "whitenoise.storage.CompressedManifestStaticFilesStorage"    

# 배포를 위한 설정 URL static 으로 접근시 아래 폴더
# python manager.py collectstatic
STATIC_ROOT = os.path.join(BASE_DIR, "staticfiles")
```

### 3. React 세팅

```bash
cd .. && cd frontend
```
> #### /frontend/package.json   
```javascript
{
...
"scripts": {
...
"relocate": "react-scripts build && rm -rf ../backend/build && mv -f build ../backend",
...
},
...
```
}

> npm run relocate

### 4.Django 세팅
> python manage.py collectstatic

### 5. 배포 설정
> DEBUG,ALLOWED_HOST 설정 (python manage.py check --deploy 로 확인 가능)
