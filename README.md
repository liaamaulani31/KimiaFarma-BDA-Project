# KimiaFarma-BDA-Project
Performance Analytics 2020-2023 - Virtual Internship Kimia Farma
Author: Lia Maulani
Date:
Descriptions:

[kf_performance_analytics.sql](https://github.com/user-attachments/files/24123202/kf_performance_analytics.sql)
--Gross Laba Calculation
WITH gross_laba AS(
  SELECT*,
    CASE
      WHEN price <=50000 THEN 0.10
      WHEN price >50000 AND price <=100000 THEN 0.15
      WHEN price >100000 AND price <=300000 THEN 0.20
      WHEN price >300000 AND price <=500000 THEN 0.25
      WHEN price >500000 THEN 0.30
    END AS gross_laba_pct
  FROM `rakamin-kf-analytics-480907.kimia_farma.kf_product`
)
--Business Metric Calculation
SELECT 
  t.transaction_id,
  t.date,
  t.branch_id,
  kc.branch_name,
  kc.kota,
  kc.provinsi,
  kc.rating AS rating_cabang,
  t.customer_name,
  t.product_id,
  p.product_name,
  p.price AS actual_price,
  t.discount_percentage,
  gl.gross_laba_pct AS persentase_gross_laba,
  p.price*(1-t.discount_percentage/100) AS nett_sales, --> Price after discount
  (p.price*(1-t.discount_percentage/100))*gl.gross_laba_pct AS nett_profit, --> Laba Bersih
  t.rating AS rating_transaksi
FROM `rakamin-kf-analytics-480907.kimia_farma.kf_final_transaction`t

--JOIN Statements
LEFT JOIN `rakamin-kf-analytics-480907.kimia_farma.kf_product` p
  ON t.product_id=p.product_id
LEFT JOIN `rakamin-kf-analytics-480907.kimia_farma.kf_kantor_cabang`kc
  ON t.branch_id=kc.branch_id
LEFT JOIN gross_laba gl
  ON t.product_id=gl.product_id
ORDER BY t.date DESC, t.transaction_id










