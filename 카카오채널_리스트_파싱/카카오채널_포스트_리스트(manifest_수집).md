# 카카오채널 포스트 리스트 `manifest.csv` 수집

이 문서는 카카오채널의 **포스트 목록 페이지만** 수집하여 후속 피드 파싱에 사용할 `manifest.csv`를 만드는 독립 기능 명세다.

## 1. 기능 범위

대상 목록 URL:

```text
https://pf.kakao.com/_XIexbj/posts
```

수행할 작업:

1. `/posts` 목록 페이지에 접속한다.
2. 동적 로딩과 무한 스크롤을 처리한다.
3. 목록에 실제로 노출된 게시물의 링크와 제목만 수집한다.
4. 최신 게시물부터 화면 노출 순서를 보존한다.
5. 목록 카드에서 상품명과 품번을 파싱한다.
6. 정확히 6개 열을 가진 `manifest.csv`만 생성한다.

수행하지 않을 작업:

- 개별 `post_url` 상세페이지 접속
- 상세 HTML 저장
- 본문 텍스트 파싱
- 이미지 URL 파싱 또는 이미지 다운로드
- 상품별 폴더 생성
- `manifest.csv` 이외의 결과 파일 생성

## 2. 입력값

기본 입력:

```yaml
channel_posts_url: "https://pf.kakao.com/_XIexbj/posts"
output_csv: "kakao-channel-products/manifest.csv"
end_post_id: ""
max_posts: 0
overwrite: false
```

입력 설명:

- `channel_posts_url`: 반드시 대상 채널의 `/posts` URL이어야 한다.
- `output_csv`: 생성할 `manifest.csv` 경로다.
- `end_post_id`: 선택값. 지정하면 해당 게시물을 결과에 포함한 뒤 수집을 종료한다.
- `max_posts`: 안전 한도다. `0`이면 별도 개수 제한 없이 목록 끝 또는 `end_post_id`까지 수집한다.
- `overwrite`: 기본값은 `false`다. 기존 파일이 있으면 임의로 덮어쓰지 않고 사용자에게 확인한다.

특정 게시물까지 수집하려면 다음 입력을 사용할 수 있다.

```yaml
end_post:
  url: "https://pf.kakao.com/_XIexbj/112603732"
  post_id: "112603732"
  product_name: "접이지사 벙거지"
  product_code: "6B501"
```

`end_post`가 제공되면 URL 끝 번호와 `post_id`가 같은지 먼저 검증한다. 상품명과 품번은 목록 카드의 마지막 항목 검수에만 사용하며 상세페이지에는 접속하지 않는다.

## 3. 실행 전 검증

1. `channel_posts_url`이 `https://pf.kakao.com/<채널ID>/posts` 형식인지 확인한다.
2. 잘못된 따옴표나 끝의 불필요한 `'` 문자를 제거하지 말고 입력 오류로 보고한다.
3. `end_post.url`이 있으면 같은 채널인지 확인한다.
4. `end_post.url` 끝 번호와 `end_post.post_id`가 같은지 확인한다.
5. 출력 파일명이 `manifest.csv`인지 확인한다. `.cvs`가 아니라 `.csv`가 올바른 확장자다.
6. 전체 수집 전에 최근 게시물 5~10개 이하로 목록 구조를 읽기 전용 확인한다.

## 4. 목록 로딩과 수집

1. `channel_posts_url`에 접속한다.
2. 문서의 `readyState=complete`와 실제 게시물 카드 생성을 모두 기다린다.
3. 목록에 노출된 게시물 링크의 `href`를 수집한다.
4. 스크롤하기 전에 현재 보이는 새 게시물을 즉시 메모리에 누적한다.
5. 아래 조건 중 하나가 충족될 때까지 아래로 스크롤한다.

종료 조건:

- `end_post_id`를 발견했고 해당 행까지 결과에 포함함
- 실제 목록 끝에 도달함
- `max_posts` 안전 한도에 도달함
- 여러 번 연속 스크롤해도 신규 `post_id`가 추가되지 않음
- 접근 제한, CAPTCHA 또는 페이지 구조 변경을 발견함

가상화된 목록은 이전 카드가 DOM에서 사라질 수 있으므로 최종 DOM만 한 번에 읽지 않는다. 새 카드를 발견할 때마다 누적한다.

게시물 번호를 증가·감소시키며 URL을 추측하지 않는다. 목록에서 실제로 발견한 `href`만 사용한다.

## 5. 게시물 식별

각 목록 카드에서 다음 값을 만든다.

- `post_id`: 게시물 `href` 또는 URL 마지막의 숫자
- `post_url`: 상대 `href`를 `https://pf.kakao.com` 기준의 절대 URL로 변환
- `list_title`: 목록 카드에 표시된 제목 원문

`post_id`를 고유키로 사용하여 중복을 제거한다. 고정 게시물이 중복 노출돼도 첫 번째 화면 노출 위치만 보존한다.

## 6. 상품명과 품번 파싱

상세페이지에 들어가지 않고 목록 카드의 `list_title`과 카드 내부 텍스트만 사용한다.

`product_name` 규칙:

1. 목록 카드의 대표 제목 또는 첫 번째 유효한 상품명 문구를 사용한다.
2. 앞뒤 공백과 연속 공백만 정리한다.
3. 맞춤법이나 표현을 임의로 수정하지 않는다.
4. 반복된 동일 상품명은 한 번만 저장한다.

`product_code` 규칙:

1. 상품명 주변의 독립된 영문·숫자 조합을 우선한다.
2. 길이를 고정하지 않는다.
3. 전화번호, 가격, 날짜, 치수는 품번에서 제외한다.
4. 후보가 없거나 여러 개라 확정할 수 없으면 빈 값으로 둔다.
5. 값을 만들기 위해 상세 게시물에 접속하지 않는다.

목록만으로 파싱할 수 없는 값은 빈 문자열로 저장한다. 이 기능의 책임은 목록 수집이며 상세 정보 보완은 후속 피드 파싱 기능의 책임이다.

## 7. `manifest.csv` 스키마

열은 아래 6개만, 아래 순서 그대로 생성한다.

```text
order,post_id,post_url,list_title,product_name,product_code
```

열 정의:

| 열 | 설명 |
|---|---|
| `order` | 목록의 최신순 노출 순번. 첫 행은 `1` |
| `post_id` | 카카오 게시물 고유번호 |
| `post_url` | 게시물 절대 URL |
| `list_title` | 목록에 표시된 원본 제목 |
| `product_name` | 목록 카드에서 파싱한 상품명 |
| `product_code` | 목록 카드에서 파싱한 품번 |

정상 데이터 예시:

```csv
order,post_id,post_url,list_title,product_name,product_code
1,113800568,https://pf.kakao.com/_XIexbj/113800568,어드밴쳐 긴챙 등산모,어드밴쳐 긴챙 등산모,6B524
```

다음 열은 생성하지 않는다.

```text
folder_name
html_file
text_file
image_count_found
image_count_saved
parse_status
download_status
overall_status
error_message
discovered_at
collected_at
processed_at
```

## 8. CSV 저장 규칙

1. 결과 파일명은 반드시 `manifest.csv`로 한다.
2. Windows Excel에서 한글이 깨지는 것을 줄이기 위해 UTF-8 BOM으로 저장한다.
3. 쉼표, 큰따옴표 또는 줄바꿈이 포함된 값은 CSV 규칙에 맞게 큰따옴표로 감싼다.
4. 모든 행의 열 개수가 헤더의 6개 열과 같은지 다시 읽어 검증한다.
5. `post_id` 중복이 없는지 확인한다.
6. `order`가 `1`부터 끊김 없이 증가하는지 확인한다.
7. 각 `post_url` 끝 번호가 `post_id`와 같은지 확인한다.

## 9. 기존 파일 처리

`overwrite=false`이고 기존 `manifest.csv`가 있으면 자동으로 삭제하거나 덮어쓰지 않는다.

사용자에게 다음 중 하나를 확인한다.

- 기존 파일을 백업하고 새로 수집
- 기존 파일에 없는 `post_id`만 병합
- 작업 취소

병합을 선택하면 `post_id`로 중복을 제거하고 현재 목록의 최신순 순서에 맞춰 `order`를 다시 부여한다.

## 10. 오류 처리

다음 상황에서는 추측하여 강행하지 않는다.

- 게시물 카드나 링크 선택자를 찾을 수 없음
- 로그인, 접근 제한 또는 CAPTCHA 발생
- `end_post_id`를 목록 끝까지 찾지 못함
- 수집 도중 신규 게시물 로딩이 반복해서 멈춤
- 목록 순서가 최신순인지 확인할 수 없음

부분 수집 결과가 있더라도 사용자에게 상태를 먼저 보고한다. 사용자가 승인하기 전 불완전한 결과를 정상 완료 파일처럼 확정하지 않는다.

## 11. 완료 조건

다음을 모두 확인한 경우에만 완료로 보고한다.

- `manifest.csv` 이외의 수집 결과 파일을 만들지 않음
- 정확히 6개 열만 존재함
- 목록에서 발견한 실제 게시물 URL만 포함함
- 중복 `post_id`가 없음
- 최신순 화면 노출 순서가 보존됨
- 종료 게시물을 지정한 경우 해당 행이 포함됨
- UTF-8 BOM과 CSV 구조 검증을 통과함

완료 보고에는 다음만 포함한다.

- 수집된 게시물 수
- 첫 번째와 마지막 `post_id`
- 종료 게시물 발견 여부
- 상품명 또는 품번이 빈 행의 수
- 생성된 `manifest.csv` 절대 경로

## 12. 재사용 요청문

다른 Codex 작업에서 이 문서와 함께 다음처럼 요청한다.

```text
첨부한 `카카오채널_포스트_리스트_manifest_수집.md` 명세대로
https://pf.kakao.com/_XIexbj/posts 목록을 수집해줘.

output_csv: C:/작업폴더/manifest.csv
end_post_id: 112603732
max_posts: 0
overwrite: false

개별 게시물 상세페이지에는 들어가지 말고 manifest.csv만 생성해줘.
먼저 최근 게시물 5개로 목록 구조를 확인한 뒤 결과를 보여줘.
```
