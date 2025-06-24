### ✅ Câu hỏi – The Graph + NFT trên Base Sepolia

**Prompt:**

> Em hãy nhờ Trea hỗ trợ tạo một dự án TypeScript cho phép người dùng nhập địa chỉ ví EVM (trên mạng Base Sepolia) để kiểm tra xem ví đó có sở hữu NFT từ BaseCamp không. Dự án cần sử dụng subgraph từ The Graph. Em cần mô tả lại cách hoạt động và mục đích của từng phần trong dự án.

---

## 🎯 **Bài thực hành: Kiểm tra ví có sở hữu NFT từ BaseCamp hay không**

**Mục tiêu:**

* Biết cách sử dụng ngôn ngữ TypeScript để xây dựng một ứng dụng nhỏ.
* Biết cách truy vấn dữ liệu từ mạng blockchain bằng The Graph.
* Kiểm tra xem một địa chỉ ví có sở hữu NFT từ dự án BaseCamp hay không.

---

## 📌 **Yêu cầu bài làm**

1. Viết một ứng dụng bằng TypeScript.
2. Cho phép người dùng nhập địa chỉ ví EVM (trên mạng Base Sepolia).
3. Dùng The Graph để truy vấn dữ liệu NFT của ví từ Subgraph của BaseCamp.
4. Nếu có NFT, hiển thị số lượng NFT và thông báo "Đã sở hữu NFT BaseCamp".
5. Nếu không có, hiển thị "Chưa sở hữu NFT BaseCamp".

---

## 🛠 **Công cụ & thư viện đề xuất**

* Node.js + TypeScript
* `graphql-request`
* Subgraph URL (giả lập): `https://api.studio.thegraph.com/query/basecamp-nft/subgraph/0.0.1`

---

## 📁 **Cấu trúc thư mục đề xuất**

```
basecamp-nft-checker/
├── src/
│   └── index.ts
├── package.json
├── tsconfig.json
```

---

## ✅ **Bước 1 – Khởi tạo dự án**

```bash
mkdir basecamp-nft-checker
cd basecamp-nft-checker
npm init -y
npm install typescript graphql-request
npx tsc --init
```

---

## ✅ **Bước 2 – Viết mã kiểm tra NFT trong `src/index.ts`**

```ts
import { request, gql } from 'graphql-request';
import readline from 'readline';

const SUBGRAPH_URL = 'https://api.studio.thegraph.com/query/basecamp-nft/subgraph/0.0.1';

const queryNFTBalance = async (walletAddress: string) => {
  const query = gql`
    query GetNFTs($owner: String!) {
      tokens(where: { owner: $owner }) {
        id
        tokenID
      }
    }
  `;

  try {
    const data = await request(SUBGRAPH_URL, query, {
      owner: walletAddress.toLowerCase()
    });

    const nftCount = data.tokens.length;

    if (nftCount > 0) {
      console.log(`✅ Ví này đã sở hữu ${nftCount} NFT từ BaseCamp.`);
    } else {
      console.log(`❌ Ví này chưa sở hữu NFT nào từ BaseCamp.`);
    }
  } catch (error) {
    console.error('Lỗi khi truy vấn dữ liệu:', error);
  }
};

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

rl.question('👉 Nhập địa chỉ ví EVM để kiểm tra: ', (address) => {
  queryNFTBalance(address).finally(() => rl.close());
});
```

---

## ✅ **Bước 3 – Chạy thử chương trình**

```bash
npx ts-node src/index.ts
```

📌 **Kết quả mong muốn:**

```
👉 Nhập địa chỉ ví EVM để kiểm tra: 0x123...
✅ Ví này đã sở hữu 2 NFT từ BaseCamp.
```

---

## 🧠 **Giải thích**

| Thành phần                         | Vai trò                      |
| ---------------------------------- | ---------------------------- |
| `readline`                         | Nhập địa chỉ ví từ bàn phím  |
| `graphql-request`                  | Gửi truy vấn tới The Graph   |
| `queryNFTBalance()`                | Hàm xử lý kiểm tra NFT       |
| `tokens(where: { owner: $owner })` | Lọc NFT theo địa chỉ ví      |
| `data.tokens.length`               | Đếm số lượng NFT có trong ví |

---

## 📚 Gợi ý nâng cao

* Hiển thị hình ảnh NFT (nếu có metadata).
* Giao diện web (dùng React + Vite).
* Kiểm tra nhiều ví cùng lúc từ tệp `.csv`.

---
