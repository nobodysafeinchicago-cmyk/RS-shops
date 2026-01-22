# RS-Shops v2.2 🛒

**A feature-rich, secure shop system for QBCore servers**

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Version](https://img.shields.io/badge/version-2.2.0-green.svg)

---

## 📋 Features

✅ **Two Shop Types:**
- **Buy Shops** - Players purchase items (24/7, Gun Stores, etc.)
- **Sell Shops** - Players sell items to shops (Pawn Shops, Black Market)

✅ **Shop Ownership System:**
- Assign shops to specific players by CitizenID
- Owners can manage their shops without admin permissions
- Optional system (can be disabled in config)

✅ **Custom Currency System:**
- Cash
- Bank
- Custom items (e.g., blood_money for illegal shops)

✅ **Advanced Features:**
- Metadata support (gun serials, quality, durability)
- Optional peds (use zones for performance)
- ox_lib context menus for easy management
- Full player synchronization
- Transaction logging

✅ **Security:**
- All transactions server-side validated
- Permission-based access control
- No client authority over prices/items
- ACE permission system

---

## 🎥 Preview

*https://youtu.be/BgJsPpj6cQ4?si=Fc5EiUln8F_qbaeI*

---

## 📦 Dependencies

**Required:**
- [qb-core](https://github.com/qbcore-framework/qb-core)
- [ox_lib](https://github.com/overextended/ox_lib)
- [ox_inventory](https://github.com/overextended/ox_inventory)
- [ox_target](https://github.com/overextended/ox_target)
- [oxmysql](https://github.com/overextended/oxmysql)

---

## 🚀 Installation

### 1. Download & Extract
```bash
cd resources
git clone https://github.com/RubeniaSalas/rs-shops.git
```

### 2. Add to server.cfg
```cfg
ensure ox_lib
ensure ox_target
ensure qb-core
ensure ox_inventory
ensure rs-shops
```

### 3. Import SQL
Run `installation.sql` in your database (phpMyAdmin, HeidiSQL, etc.)

This will create:
- `rs_shops` - Shop data
- `rs_shop_items` - Items shops sell
- `rs_shop_sell_items` - Items shops buy (pawn)
- `rs_shop_txn` - Transaction logs

### 4. Set Permissions
Add to your `server.cfg`:
```cfg
# RS-Shops Admin Permission
add_ace group.admin rs.shops allow
```

### 5. Restart Server
```
restart rs-shops
```

---

## 🎮 Usage

### Player Commands

**None** - Players interact with shop peds/zones directly

### Admin Commands

| Command | Description | Permission |
|---------|-------------|------------|
| `/createshop` | Create a shop at your location | rs.shops |
| `/shopmanager` | Manage nearest shop | rs.shops or owner |
| `/reloadshops` | Reload all shops | rs.shops | rarely used as shops be working right away.

---

## 🛠️ Creating Your First Shop

### Example: 24/7 Store (Buy Shop)

1. Stand where you want the shop
2. Type `/createshop`
3. Fill in:
   - **Name:** 24/7 Downtown
   - **Type:** Buy Shop
   - **Owner:** *(leave blank for admin-only)*
   - **Currency:** Cash
   - **Use Ped:** ✓ Checked

4. Type `/shopmanager` near the shop
5. Select "Manage Shop Items"
6. Add items:
   - water - $5
   - sandwich - $10
   - phone - $500

Done! Players can now buy from your shop.

---

### Example: Pawn Shop (Sell Shop)

1. Stand where you want the shop
2. Type `/createshop`
3. Fill in:
   - **Name:** Downtown Pawn
   - **Type:** Sell Shop
   - **Owner:** *(optional player CitizenID)*
   - **Currency:** Cash
   - **Use Ped:** ✓ Checked

4. Type `/shopmanager` near the shop
5. Select "Manage Sell Items"
6. Add items shop will buy:
   - goldbar - Shop pays $500
   - diamond - Shop pays $1000
   - rolex - Shop pays $750

Done! Players can now sell these items to your shop.

---

### Example: Illegal Shop (Custom Currency)

1. Type `/createshop`
2. Fill in:
   - **Name:** Black Market
   - **Type:** Buy Shop
   - **Currency Type:** Custom Item
   - **Currency Item:** blood_money/diamonds
   - **Use Ped:** ✗ Unchecked *(zone only for stealth)*

3. Add items like weapons, lockpicks, etc.

Players will need blood_money to purchase from this shop!

---

## ⚙️ Configuration

Edit `config.lua`:

```lua
Config.Debug = false  -- Enable for troubleshooting
Config.DefaultPedModel = 'mp_m_shopkeep_01'  --in game you can pick any ped you like. You can visit https://docs.fivem.net/docs/game-references/ped-models/ for ped models.
Config.InteractionDistance = 2.0
Config.AllowOwnership = true  -- Enable shop ownership
Config.DefaultSellMultiplier = 0.50  -- 50% of item value
```

---

## 📊 Shop Types Explained

### Buy Shop (Players Purchase)
- Players interact: **"Open Shop"**
- Shop sells items TO players
- Example: 24/7, Gun Store, Clothing Store

### Sell Shop (Players Sell - Pawn)
- Players interact: **"Sell Items"**
- Shop buys items FROM players
- Example: Pawn Shop, Black Market
- Only shows items player has that shop accepts

---

## 🔐 Ownership System

### No Owner (Public Shop)
```
Owner CitizenID: [leave blank] --You can add owner by citizenID and they wiill earn a commision for the purchase items for that shop.
```
- Any admin can manage
- Good for server-controlled shops

### Player-Owned Shop
```
Owner CitizenID: ABC12345
```
- Only owner + admins can manage
- Owner gets notifications on sales
- Perfect for player businesses

### Disable Ownership
```lua
Config.AllowOwnership = false
```
- Reverts to admin-only management

---

## 💰 Currency Types

| Type | Description | Example Use |
|------|-------------|-------------|
| Cash | Player's cash | Legal shops |
| Bank | Player's bank account | Premium stores |
| Custom Item | Specific item (e.g., blood_money, diamonds, etc) | Illegal shops, gang vendors |

---

## 📝 Database Structure

### rs_shops
Stores shop data (location, owner, currency type)

### rs_shop_items
Items shop SELLS (for buy shops)

### rs_shop_sell_items
Items shop BUYS (for sell/pawn shops)

### rs_shop_txn
Transaction log for auditing

---

## 🐛 Troubleshooting

### "No shop nearby"
- Stand closer (default radius: 7.5 units)
- Use `/reloadshops` to force sync

### SQL errors on startup
- Ensure oxmysql is running
- Run `installation.sql` manually
- Check DB user has CREATE/ALTER permissions

### Players see wrong interaction
- Edit shop via `/shopmanager`
- Change "Shop Type" to correct type
- Restart rs-shops

### Currency item not working
- Ensure item exists in ox_inventory
- Check spelling of currency_item
- Verify player has the item

### Peds not spawning
- Check F8 for model errors
- Verify ox_target is running
- Ensure "Use Ped" is checked in shop settings

---

## 🔄 Updating from v2.0/v2.1

1. Backup your database
2. Replace all script files
3. Run `installation.sql` (safe, won't delete data)
4. Restart rs-shops

Your existing shops will work with defaults:
- `currency_type = 'cash'`
- `use_ped = 1`
- No owner

---

## 📚 Documentation

Full guides included:
- `RS-SHOPS_COMPLETE_GUIDE.md` - Comprehensive usage guide
- `RS-SHOPS_OWNERSHIP_SELLING_GUIDE.md` - Ownership & pawn system details

---

## 🤝 Support

**Before asking for help:**
1. Check F8 console for errors
2. Verify all dependencies are updated
3. Ensure ACE permissions are set
4. Try `/reloadshops`

**For issues:**
- Open a GitHub Issue with:
  - Error logs
  - Server info (FiveM version, framework version)
  - Steps to reproduce

---

## 📜 License

This project is licensed under the **MIT License**.

```
Copyright (c) 2025 RubeniaSalas

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
```

**Please do not:**
- Remove credits
- Resell as your own
- Claim original authorship

**You are free to:**
- Modify for your server
- Share with others (with credit)
- Create pull requests

---

## 🌟 Credits

**Author:** RubeniaSalas  
**Repository:** https://github.com/nobodysafeinchicago-cmyk/rs-shops  
**Version:** 2.2.0  

Special thanks to:
- QBCore Framework Team
- Overextended (ox_lib, ox_inventory, ox_target)
- FiveM Community

---

## 🎯 Roadmap

**Possible future features:**
- [ ] ESX support
- [ ] Discord webhooks
- [ ] Shop analytics dashboard
- [ ] Multi-language support
- [ ] Stock auto-restock system
- [ ] Employee system

**Suggestions welcome!** Open an issue with feature requests.

---

## ⭐ Show Your Support

If you like this script:
- ⭐ Star this repository
- 🐛 Report bugs
- 💡 Suggest features
- 🔀 Create pull requests
- If you have any questions or would like to get something edited you are more than welcome to come to our server discord and open a ticket I will assist the most I can. Discord: https://discord.gg/7Emnf2dd3A
---

**Made with ❤️ for the FiveM community**
