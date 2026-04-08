# Todo List
* [ ] s



# Flow Rest API Register 

```mermaid
flowchart LR;
A[Client<br/>POST /api/v1/auth/register] --> B[Router<br/>routes.go]
B --> C[Handler<br/>handleRegister]
C --> D[Service<br/>register]
D --> E[Repository<br/>CreateUser]
E --> F[(Database)]
F --> E
E --> D
D --> C
C --> G[Response JSON<br/>201 Created]
```


## Flow Split bill
1. Tambah bill
2. Isi nama bill/tagihan
3. isi siapa yang ikut (ini aku masih bingung dibelakangnya disatuin sama tabel tagihan atau tidak)
4. Tambah item menu yang dipesan(disini isi nama item, qty, harga satuan, punya siapa)
5. jika sudah ada opsi menggunakan ppn atau tidak
6. jika menggunakan ppn  masukan nama pajak persentasenya
7. lalu ada opsi biaya lainnya jika ada, prosesnya harusnya sama dengan feature ppn
8. lalu pilih ringkasan untuk menampilkan hasilnya
9. pada ringkasan itu terdapat total yang harusu dibayar, subtotal(total tanpa pajak), total ppn, total biaya lainnya
10. lalu pada ringkasan terdapat section tagihan per orang 
11. lalu ada section rincian semua item, subtotal, ppn, biaya lainnya, dan total semuanya
12. lalu ada feature simpan dan bagikan rinciannya (tapi harusnya udah disimpan saat dia klik detail ringkasan)
13. jika sudah balik ke home lagi dengan menampilkan riwayat split bill

# ERD
```mermaid
erDiagram
    USER{
        uuid id PK
        string first_name
        string last_name
        string email
        string avatar_url
        timestamp created_at
        timestamp updated_at
    }

    BILL{
        uuid id PK
        uuid created_by FK
        string title
        date bill_date
        boolean is_tax
        boolean is_extra_fee
        timestamp created_at
    }

    ITEM{
        uuid id PK
        uuid bill_id FK
        string name
        int qty
        int unit_price
        int subtotal
    }

    ITEM_PARTICIPANT{
        uuid id PK
        uuid item_id FK
        uuid participant_id FK
        decimal portion "ini fungsi untuk bagi rata"
    }

    PARTICIPANT{
        uuid id PK
        uuid bill_id FK
        string name
    }

    TAX{
        uuid id PK
        uuid bill_id FK
        string label
        decimal percentage
    }

    EXTRA_FEE{
        uuid id PK
        uuid billl_id FK
        string label
        string fee_type "ENUM: PERCENTAGE | FLAT"
        decimal percentage
        decimal flat_amount
    }

    BILL_SUMMARY{
        uuid id PK
        uuid bill_id FK
        int subtotal
        int total_tax
        int total_extra_fee
        int grand_total
        timestamp calculated_at
    }

    PARTICIPANT_SUMMARY{
        uuid id PK
        uuid bill_summary_id FK
        uuid participant_id FK
        int items_total
        int tax_share
        int fee_share
        int total_due
    }

    USER ||--o{ BILL : has
    BILL ||--o{ ITEM: contains
    ITEM ||--o{ ITEM_PARTICIPANT : has
    BILL ||--o{ PARTICIPANT : contains
    BILL ||--o{ BILL_SUMMARY : has
    BILL_SUMMARY ||--o{ PARTICIPANT_SUMMARY: has
    PARTICIPANT ||--o{ ITEM_PARTICIPANT : has
    PARTICIPANT ||--o{ PARTICIPANT_SUMMARY : contains
    BILL ||--o{ TAX : has
    BILL ||--o{ EXTRA_FEE : has
```
