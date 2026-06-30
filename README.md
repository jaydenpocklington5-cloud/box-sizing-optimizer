# 📦 Box-Sizing Optimizer

A browser app that looks at historic order data and works out the most
cost-efficient shipping box for every order — then recommends the smallest
line-up of box sizes to stock.

**Live app:** https://jaydenpocklington5-cloud.github.io/box-sizing-optimizer/

Everything runs client-side in your browser. Nothing is uploaded to a server.

## Features

- **Recommendation** of the minimum number of distinct box sizes to stock.
- **Override** the desired number of box sizes (1–10) and watch cost + coverage update.
- **3D order viewer** — a transparent box showing the items packed inside.
- **Build & test an order** sandbox — assemble a custom order (or roll a random one)
  and see which box it needs.
- **Upload your own CSVs** (orders / product catalogue / box catalogue), or use the
  built-in sample data.

## Input files

| File | Required | Columns |
|------|----------|---------|
| Orders | yes | Shopify export (`Name`, `Lineitem sku`, `Lineitem quantity`) or `order_id,sku,quantity` |
| Product catalogue | recommended | `sku,name,length,width,height,weight` |
| Box catalogue | optional | `name,length,width,height,cost,max_weight` |

Dimensions default to cm, weights to kg (both selectable). SKUs missing from the
catalogue are estimated from weight.

## How it works

1. Group order lines into orders and attach each item's size/weight.
2. For each order find boxes that fit (every item fits + total volume ≤ box × fill limit).
3. Cost each option on the greater of actual vs dimensional weight, so snug boxes win.
4. Recommend a box line-up via a two-phase greedy search (reach coverage, then cut cost).
