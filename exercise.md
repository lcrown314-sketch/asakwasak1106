# Git 실습 Exercise

이 문서는 지금까지 진행한 Git 실습을 다시 따라 하며 복습할 수 있도록 정리한 가이드입니다.

## 학습 목표

- 브랜치 생성, 이동, 이름 변경을 익힙니다.
- 커밋 생성과 `--amend` 수정을 익힙니다.
- 브랜치별 변경 내용을 비교합니다.
- 최종적으로 `release` 브랜치를 `main` 브랜치로 보내는 Pull Request를 만듭니다.

## 전체 커밋 그래프

```text
* 80e19ae (release) 5.txt
| * ce27441 (feat/awesome2) 4.txt a2
| | * 7c96ae6 (feat/awesome1) 4.txt a1
| |/  
|/|   
* | ea0aa66 (main) 4.txt main
|/  
| * 6b30fd8 (fix, feat/login) 4.txt
| * 9552719 3.txt
|/  
| * b4192c5 (dev) 2.txt
|/  
* 9a825a8 1.txt
```

## 브랜치와 커밋 요약

| 브랜치 | 커밋 | 주요 변경 |
| --- | --- | --- |
| `main` | `9a825a8` | `1.txt` 추가 |
| `main` | `ea0aa66` | `4.txt`에 `hello1`, `hello2` 추가 |
| `dev` | `b4192c5` | `2.txt` 추가 |
| `fix` | `9552719` | `3.txt` 추가 |
| `fix` | `6b30fd8` | 빈 `4.txt` 추가 |
| `feat/login` | `6b30fd8` | `fix`와 같은 커밋 공유 |
| `feat/awesome1` | `7c96ae6` | `4.txt` 첫 줄 `hello1` → `bye3` |
| `feat/awesome2` | `ce27441` | `4.txt` 추가 후 커밋 메시지/내용 amend |
| `release` | `80e19ae` | `4.txt` 최종 변경, `5.txt` 추가 |

## 단계별 실습

### 실습 1. 저장소 초기화와 첫 커밋

**목표**: 빈 파일을 만들고 첫 커밋을 남깁니다.

```bash
git init
touch 1.txt
git add 1.txt
git commit -m "1.txt"
```

**예상 결과**

- `main` 브랜치에 `1.txt`가 추가됩니다.
- 커밋 해시는 `9a825a8`입니다.

**확인**

```bash
git log --oneline
git show --stat HEAD
```

### 실습 2. dev 브랜치 생성과 브랜치명 변경

**목표**: 브랜치를 만들고, 이름을 바꾸며 이동합니다.

```bash
git switch -c dev
touch 2.txt
git add 2.txt
git commit -m "2.txt"

# 연습으로 브랜치 이름 변경: dev2 -> dev
git branch -m dev2 dev
```

**예상 결과**

- `dev` 브랜치에 `2.txt`가 추가됩니다.
- 현재 브랜치는 `dev`입니다.

**확인**

```bash
git branch
git log --oneline dev
```

### 실습 3. fix 브랜치에서 연속 커밋

**목표**: 한 브랜치에서 여러 파일을 순서대로 커밋합니다.

```bash
git switch -c fix main
touch 3.txt
git add 3.txt
git commit -m "3.txt"

touch 4.txt
git add 4.txt
git commit -m "4.txt"
```

**예상 결과**

- `3.txt`, 빈 `4.txt`가 `fix` 브랜치에 커밋됩니다.
- 최신 커밋 해시는 `6b30fd8`입니다.

### 실습 4. feat/login 브랜치 만들기

**목표**: 기존 커밋을 공유하는 새 브랜치를 만듭니다.

```bash
git switch -c feat/login fix
```

**예상 결과**

- `feat/login`은 `fix`의 최신 커밋 `6b30fd8`을 가리킵니다.
- 두 브랜치는 같은 위치에서 시작합니다.

**확인**

```bash
git rev-parse fix feat/login
```

### 실습 5. main에서 4.txt 작성

**목표**: `main`에 직접 커밋하여 브랜치별 차이를 만듭니다.

```bash
git switch main
cat > 4.txt <<'EOF'
hello1
hello2
EOF
git add 4.txt
git commit -m "4.txt main"
```

**예상 결과**

- `main`의 `4.txt`는 2줄입니다.
- 커밋 해시는 `ea0aa66`입니다.

### 실습 6. feat/awesome1에서 변경 커밋

**목표**: 기존 파일의 일부만 바꿔 커밋합니다.

```bash
git switch -c feat/awesome1 main
sed -i '' '1s/hello1/bye3/' 4.txt
git add 4.txt
git commit -m "4.txt a1"
```

**예상 결과**

- `feat/awesome1`의 `4.txt` 첫 줄이 `bye3`입니다.
- 커밋 해시는 `7c96ae6`입니다.

### 실습 7. feat/awesome2에서 amend 연습

**목표**: 커밋 후 내용 또는 메시지를 `--amend`로 수정합니다.

```bash
git switch -c feat/awesome2 9a825a8
cat > 4.txt <<'EOF'
hello1
hello2
EOF
git add 4.txt
git commit -m "4.txt a2 (임시)"
git commit --amend -m "4.txt a2"
```

**예상 결과**

- 마지막 커밋 메시지가 `4.txt a2`로 바뀝니다.
- 커밋 해시는 `ce27441`입니다.

### 실습 8. release 브랜치와 최종 파일 준비

**목표**: 최종 릴리즈 변경을 만들고 PR 준비 상태를 만듭니다.

```bash
git switch -c release main
printf 'sayonara4\nhello2\nnihao5' > 4.txt
touch 5.txt
git add 4.txt 5.txt
git commit -m "5.txt"
```

**예상 결과**

- `release`의 `4.txt`는 최종 3줄이며 마지막 줄에 개행이 없습니다.
- `5.txt`가 추가됩니다.
- 커밋 해시는 `80e19ae`입니다.

### 실습 9. 차이 확인과 PR 생성

**목표**: `release`와 `main` 차이를 확인하고 PR을 만듭니다.

```bash
git diff main..release
gh pr create --base main --head release \
  --title "release: merge release into main" \
  --body "Release changes prepared for merge."
```

**예상 결과**

- `main..release` 비교에서 `4.txt`, `5.txt` 변경이 보입니다.
- GitHub에 PR이 생성됩니다.

## 복습 문제

1. 브랜치 이름을 `dev2`에서 `dev`로 바꾸는 명령은 무엇인가요?
2. `git commit --amend`로 마지막 커밋의 어떤 부분을 수정할 수 있나요?
3. `fix`와 `feat/login`이 같은 해시를 가리키는 이유는 무엇인가요?
4. `release` 브랜치에서 `4.txt`의 최종 내용은 무엇인가요?
5. 전체 브랜치 그래프를 출력하는 명령은 무엇인가요?

## 정답 포인트

1. `git branch -m dev2 dev`
2. 마지막 커밋 메시지와 staging된 변경 내용
3. `feat/login`이 `fix`의 최신 커밋에서 생성되었기 때문입니다.
4. `sayonara4`, `hello2`, `nihao5`이며 마지막 줄 뒤 개행이 없습니다.
5. `git log --all --graph --oneline --decorate`

## 자주 쓰는 확인 명령

```bash
git status
git branch -a
git log --all --graph --oneline --decorate
git diff main..release
git diff main...release
```
