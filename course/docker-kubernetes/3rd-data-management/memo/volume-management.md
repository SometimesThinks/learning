# Docker 볼륨 마운트 전략

## 명령어

```bash
docker run -d --rm -p 3000:80 --name feedback-app \
  -v feedback:/app/feedback \
  -v "/Users/yundaeyeong/Desktop/learning/course/docker-kubernetes/3rd-data-management/nodejs-app:/app:ro" \
  -v /app/temp \
  -v /app/node_modules \
  feedback-node:volumes
```

## 볼륨 마운트 분석

### 1. Named Volume: `/app/feedback`

- `-v feedback:/app/feedback`
- **역할**: 피드백 데이터를 영속적으로 저장
- **특징**: 컨테이너 삭제 후에도 데이터 유지

### 2. Bind Mount: 프로젝트 폴더 전체

- `-v ".../nodejs-app:/app:ro"`
- **역할**: 로컬 코드 변경사항을 컨테이너에 반영
- **특징**: 읽기 전용(`:ro`)으로 설정하여 컨테이너에서 파일 수정 방지

### 3. Anonymous Volume: `/app/temp`

- `-v /app/temp`
- **역할**: 임시 파일 저장 공간
- **특징**: 컨테이너 삭제 시 함께 삭제됨

### 4. Anonymous Volume: `/app/node_modules`

- `-v /app/node_modules`
- **역할**: 로컬 `node_modules`와 분리하여 컨테이너 내부의 `node_modules` 사용
- **특징**: 로컬 패키지 의존성과 충돌 방지

## 동작 원리

Docker는 **더 구체적인 경로 마운트가 우선순위가 높습니다**:

- 전체 `/app` 폴더를 바인드 마운트하지만
- `/app/feedback`, `/app/temp`, `/app/node_modules`는 각각의 볼륨 마운트가 우선 적용됨
- 이로 인해 해당 경로들은 바인드 마운트가 아닌 볼륨을 사용

## 결과

✅ **로컬 코드 변경사항이 컨테이너에 실시간 반영** (읽기 전용)  
✅ **피드백 데이터는 영속적으로 저장** (named volume)  
✅ **임시 파일은 컨테이너 내부에서만 사용** (익명 볼륨)  
✅ **로컬과 컨테이너의 `node_modules` 분리** (의존성 충돌 방지)
