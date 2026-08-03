# Introduction to Angular - Day 1

Aaj maine Angular ke basics, uske benefits aur setup process ke baare mein padha. Ye rahe mere notes:

## 1. Angular kya hai aur kyun use karein?
- Angular ek bohot popular JavaScript framework hai jise **Google** ne banaya hai aur maintain karta hai. Ye web apps banane ke kaam aata hai.
- **Performance:** Ye fast load hota hai, screen jaldi update karta hai. Isme code ko manage karna aasan hai (modularity), services share karne ke liye "dependency injection" hai, aur testing bhi easy hoti hai.
- **Mobile Support:** Angular se hum aisi single application bana sakte hain jo mobile aur desktop dono par achhe se chale bina kisi third-party tool ke.
- **Language:** Angular apps hum TypeScript mein likhte hain (jo sabse popular choice hai).

## 2. ECMAScript vs TypeScript
- **ECMAScript:** Ye JavaScript ka official standard naam hai. Har saal iska naya version aata hai naye features ke sath (jaise ES6/ES2015 jisme classes, modules wagaira aaye the).
- **TypeScript:** Ise Microsoft ne banaya hai. Ye JavaScript ka "superset" hai, matlab isme poori JavaScript toh hai hi, but uske upar kuch extra features hain (jaise **Types**, Interfaces, Classes). Ise run karne ke liye pehle compile karke wapas plain JavaScript mein convert kiya jata hai. Isse code likhna aur maintain karna kaafi aasan ho jata hai.

## 3. Angular Setup & Installation
1. **Node.js Install karein:** Angular Node.js pe chalta hai. (Check karne ke liye: `node -v` aur `npm -v`)
2. **Angular CLI Install karein:** Terminal mein likhein: `npm install -g @angular/cli`. (Check karne ke liye: `ng version`)
3. **First App Banayein aur Run karein:**
   - Naya project banane ke liye: `ng new my-first-app`
   - Folder mein jaane ke liye: `cd my-first-app`
   - App run karne ke liye: `ng serve`
   - Browser mein open karein: `http://localhost:4200`

## 4. Angular CLI (Command Line Interface) kya hai?
CLI ek helper tool hai jo terminal mein chalta hai. Ye hamaare liye project setup karna, naye files banana, aur app ko run karne jaise boring aur manual kaam khud kar deta hai, taaki humara focus sirf app ka code likhne par rahe.

**Most Used CLI Commands:**
- `ng new <name>` : Naya project banata hai.
- `ng serve` : App ko local machine par run karta hai aur code save karte hi auto-reload hota hai.
- `ng generate component <name>` (short: `ng g c <name>`) : Naya component banata hai uski saari files ke sath.
- `ng build` : App ko final deployment ke liye package karta hai.
- `ng test` : Tests run karta hai.

---
## Homework Tasks
- **What is ECMAScript?** Ye JavaScript language ka official naam hai, jisme har saal naye features add hote hain.
- **What is TypeScript?** Ye Microsoft ka banaya gaya JavaScript ka ek advanced version (superset) hai, jisme "Types" aur object-oriented features hote hain.
- **Why Angular CLI?** CLI isliye diya gaya hai taaki hum ghanton baith kar project files, folders aur configurations khud setup na karein. CLI ek command mein hi ek clean aur structured project ready kar deta hai, jisse hamara bohot time bachta hai.
