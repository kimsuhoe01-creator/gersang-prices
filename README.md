# gersang-prices — 거상 사냥 장부

거상(The Great Merchant)의 장터 시세 데이터(`market_prices.json`)와,
그 시세로 사냥 수익·시급을 계산하는 정적 웹앱(`index.html`)입니다.

## 사냥 장부 (index.html)

사냥 한 판의 결과를 적으면 순수익과 시급을 계산해 주는 계산기입니다.

- **전리품**: 아이템 이름 · 수량 · 개당 가격. 품목별로 「장터」(수수료 적용) / 「상점·기타」를 선택합니다.
- **지출**: 물약, 소모품, 입장료 등.
- **결산**: 판매액 − 장터 수수료 − 지출 = 순수익, 그리고 사냥 시간으로 나눈 **시급(냥/시간)**.
- **사냥터 비교**: 현재 입력을 이름 붙여 저장하고, 사냥터별 시급을 표로 비교합니다.
- 금액은 `1.5억`, `3000만`, `1억2000만`, `12,345`처럼 자유롭게 입력할 수 있습니다.
- 입력한 가격은 브라우저(localStorage)에 기억되어 다음에 같은 아이템 이름을 쓰면 자동 완성됩니다.
- 같은 폴더의 `market_prices.json`에 시세가 있으면 이름 입력 시 가격을 자동으로 채웁니다.

모든 데이터는 브라우저에만 저장되며 서버로 전송되지 않습니다.

### 실행 방법

브라우저 보안 정책 때문에 `index.html`을 파일로 직접 열면 시세 파일을 읽지 못합니다
(계산기는 그대로 동작하고, 가격만 직접 입력하면 됩니다). 시세 자동 완성까지 쓰려면:

```bash
# 로컬에서
python3 -m http.server 8000
# → http://localhost:8000
```

또는 GitHub Pages: 저장소 **Settings → Pages → Deploy from a branch → `main` / root**
설정 후 `https://<계정>.github.io/gersang-prices/` 로 접속합니다.
파일로 직접 열었다면 하단의 「시세 JSON 직접 열기」로 `market_prices.json`을 수동으로 불러올 수도 있습니다.

## 시세 데이터 (market_prices.json)

```json
{
  "schema_version": 1,
  "generated_at": "2026-08-26T12:34:00.000Z",
  "items": [
    { "name": "아이템 이름", "price": 1250000 }
  ]
}
```

- `items[].name` — 아이템 이름 (계산기의 이름 자동 완성 목록에 사용)
- `items[].price` — 개당 가격(냥)

계산기는 스키마 변화에 관대합니다: 이름은 `name`/`item_name`/`item`/`title`,
가격은 `price`/`avg_price`/`average`/`recent_price`/`median_price`/`min_price`/`value`
중 먼저 발견되는 필드를 사용하고, 그 밖의 필드는 무시합니다.
