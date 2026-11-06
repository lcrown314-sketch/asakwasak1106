# asakwasak1106

Git/GitHub 실습 기록 저장소입니다.
브랜치 생성, 커밋, 수정(amend), 병합(PR) 흐름을 직접 연습한 결과를 정리합니다.

## 학습 목표

- Python과 AI 엔지니어링을 학습합니다.
- 머신러닝/딥러닝과 금융 AI 서비스 개발에 관심이 있습니다.
- 금융과 AI 기술을 결합한 서비스를 직접 개발하는 것이 목표입니다.

## 저장소 구성

| 파일 | 설명 |
| --- | --- |
| `1.txt` | 초기 커밋에서 생성한 빈 파일 |
| `2.txt` | `dev` 브랜치 실습에서 추가한 파일 |
| `3.txt` | `fix` 브랜치 실습에서 추가한 파일 |
| `4.txt` | 브랜치별로 내용이 달라지는 주요 충돌/병합 연습 파일 |
| `5.txt` | `release` 브랜치에서 추가한 파일 |
| `README.md` | 저장소 개요와 브랜치 구조 |
| `exercise.md` | 지금까지 진행한 Git 실습을 복습하는 가이드 |

## 브랜치 구조

```
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

### 브랜치별 요약

| 브랜치 | 주요 작업 |
| --- | --- |
| `main` | 기준 브랜치. `1.txt`, `4.txt`(`hello1` / `hello2`)를 관리합니다. |
| `dev` | `2.txt`를 추가하고, `dev2` → `dev` 브랜치명 변경을 연습합니다. |
| `fix` | `3.txt`, 빈 `4.txt`를 순서대로 추가합니다. |
| `feat/login` | `fix`와 같은 커밋을 공유하는 브랜치 생성을 연습합니다. |
| `feat/awesome1` | `4.txt` 첫 줄을 `hello1` → `bye3`으로 변경합니다. |
| `feat/awesome2` | `4.txt` 추가 커밋을 `git commit --amend`로 수정합니다. |
| `release` | `4.txt` 최종 변경과 `5.txt` 추가, PR 준비를 담당합니다. |

## 공부 방법

1. `exercise.md`의 단계별 실습을 순서대로 읽습니다.
2. 각 브랜치의 변경 사항을 `git diff`로 직접 확인합니다.
3. 마지막으로 `git log --all --graph --oneline --decorate`로 전체 흐름을 복습합니다.

## 주요 명령어

```bash
git status
git branch -a
git log --all --graph --oneline --decorate
git diff main..release
```
