# HID iClass Cheatsheet
*Proxmark 3 Iceman fork*

---


### 🔍 Check Card Info
```bash
hf iclass info
```
- Look for **fingerprint**:
  - **Legacy** → cloneable
  - **Legacy Elite** → cloneable (most of the time) 
  - **SE** → try downgrade attack  
  - **SEOS** → not cloneable  

---

### 🔑 Elite vs standard 
Elite cards usually end at ```Card chip....``` and standard cards usually has extra block 7 decoder information. Commands without --elite would not work elite cards, and vice vera. Take note of the card type.
- **Elite key output example**:  
  ![Elite iCLASS](./images/eliteiclass.png)

- **Non-elite key output example**:  
  ![Non-Elite iCLASS](./images/noneliteiclass.png)

---

### 📥 Dumping Cards
- Many legacy cards use **default keys**. Try dumping with:

**Standard**
```bash
hf iclass dump --ki <0,1,2,3>
```

**Elite**
```bash
hf iclass dump --ki <0,1,2,3> --elite
```

---

### 📂 Dictionary Attacks
If non-default keys are used, find keys using dictionary attack:

**Standard**
```bash
hf iclass chk -f iclass_default_keys.dic
```

**Elite**
```bash
hf iclass chk -f iclass_elite_keys.dic --elite
```

Once you have the right keys:
```bash
hf iclass dump -k <insert keys> #include --elite if elite keyed
```

### ✍️ Cloning
After finding keys on destination tag with dictionary attack or a test dump:
```bash
hf iclass restore -f <source tag dump file> --first 6 --last 18 --ki 0 #edit dump filename and --ki accordingly
```
In most cases, you only need to clone blocks 6 to 9 for the clone to work. 

### Upgrading standard to elite keys
If copying from elite cards to standard cards, upgrade standard card to elite by writing new key to block 3:
```bash
hf iclass calcnewkey --oki X --nki Y --elite #X = key of destination fob, Y = Key from target fob
hf iclass wrbl --blk 3 -d <XOR key> --ki X #XOR Key from calcnewkey output will be the new key
#if blk 4 and 5 are different from source tag, copy block 4 and 5 to destination tag
hf iclass wrbl --blk 5 -d 1122334455667788 --key 5b7c62c491c11b39 --elite
```

### Downgrading elite to standard keys

```bash
hf icl chk -f iclass_elite_keys.dic --elite (./images/chk.png)
hf iclass calcnewkey --oki 4 --nki 1 --oldelite (./images/calc.png)
hf iclass wrbl --blk 3 -d D7DACAD37ACA9DED --ki 4 --elite (./images/wrbl.png)
```

