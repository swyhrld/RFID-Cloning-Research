# Proxmark3 Cheatsheet
*(Iceman fork)*

---

## 🔹 HID — iCLASS

### 🔍 Check Card Info
```bash
hf iclass info
```
- Look for **fingerprint**:
  - **Legacy** → cloneable  
  - **SE** → try downgrade attack  
  - **SEOS** → not cloneable  

---

### 🔑 Elite vs Non-Elite Keys
- **Elite key output example**:  
  ![Elite iCLASS](eliteiclass.png)

- **Non-elite key output example**:  
  ![Non-Elite iCLASS](noneliteiclass.png)

---

### 📥 Dumping Cards
- Many legacy cards use **default keys**. Try dumping with:

**Non-Elite key**
```bash
hf iclass dump --ki <0,1,2,3>
```

**Elite key**
```bash
hf iclass dump --ki <0,1,2,3> --elite
```

---

### 📂 Dictionary Attacks
If non-default keys are used:

**Non-Elite**
```bash
hf iclass chk -f iclass_default_keys.dic
```

**Elite**
```bash
hf iclass chk -f iclass_elite_keys.dic --elite
```

---

### 🔑 Dump With Known Keys
Once you have the right keys:
```bash
hf iclass dump -k <insert keys>
```
(use `-k` instead of `--ki 0`)
