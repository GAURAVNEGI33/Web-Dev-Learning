# JavaScript Event Loop

JavaScript ek **single-threaded** language hai, matlab yeh ek time pe sirf ek hi kaam kar sakti hai. Lekin fir bhi hum asynchronous operations (jaise setTimeout, API calls) perform kar paate hain bina main thread ko block kiye. Yeh jaadu **Event Loop** ki wajah se hota hai.

Aao isko detail mein samajhte hain!

## Architecture: Kaam Kaise Hota Hai?

JavaScript execution engine mein mainly yeh components hote hain:

1. **Call Stack (Execution Stack):**
   - Yeh wo jagah hai jahan hamara main JavaScript code execute hota hai.
   - Code lines ek-ek karke yahan aati hain aur run hoti hain (LIFO - Last In First Out).
   - Agar yahan koi sync function aata hai, toh pehle wo poora execute hota hai.

2. **Web APIs (Browser APIs / Node APIs):**
   - Jab koi asynchronous task (jaise `setTimeout`, `fetch()`, DOM events) aata hai, toh Call Stack usko Web APIs ke paas bhej deta hai.
   - Web API background mein us task ka wait karta hai (jaise timer run karna), taaki hamara main Call Stack block na ho.

3. **Callback Queue (Task Queue):**
   - Jaise hi Web API ka task complete hota hai (e.g., timer khatam ho gaya), uska callback function uth ke **Callback Queue** mein aa jata hai.

4. **Microtask Queue:**
   - Yeh Callback Queue ka VIP version hai.
   - Promises (`.then()`, `.catch()`) aur `MutationObserver` ke callbacks isme aate hain.
   - Event Loop humesha Microtask Queue ko **zyada priority** deta hai Callback Queue se.

## The Event Loop

Event Loop ka sirf ek hi kaam hai: **Monitoring**.

- Yeh lagataar check karta rehta hai:
  1. Kya **Call Stack** khali hai?
  2. Kya **Microtask Queue** ya **Callback Queue** mein koi code wait kar raha hai?

**Steps:**

1. Agar Call Stack khali ho gaya (saara synchronous code run ho chuka hai).
2. Toh Event Loop sabse pehle **Microtask Queue** mein dekhta hai. Agar wahan koi VIP callback hai, toh usko Call Stack mein bhejta hai.
3. Jab Microtask Queue poori khali ho jati hai, tab wo **Callback Queue (Task Queue)** ke paas jata hai aur wahan se ek-ek karke callbacks ko utha ke Call Stack mein daalta hai execute hone ke liye.

## Example se Samajhte hain

```javascript
console.log("1. Start"); // Call Stack

setTimeout(() => {
  console.log("2. Timer (Callback Queue)");
}, 0); // Web API -> Callback Queue

Promise.resolve().then(() => {
  console.log("3. Promise (Microtask Queue)");
}); // Microtask Queue

console.log("4. End"); // Call Stack
```

**Output kya hoga?**

```
1. Start
4. End
3. Promise (Microtask Queue)
2. Timer (Callback Queue)
```

**Aisa kyu hua?**

- `console.log("1. Start")` aur `console.log("4. End")` synchronous code hain, seedhe Call Stack mein gaye aur print ho gaye.
- Uske baad Call Stack khali ho gaya.
- `Promise` VIP hai (Microtask Queue), isliye Call Stack khali hote hi pehle yeh check hua aur print hua.
- `setTimeout` normal task hai (Callback Queue), isliye microtasks khatam hone ke baad sabse last mein run hua.

## Conclusion

Event Loop basically ek manager (infinite loop) hai jo Call Stack aur queues ke beech traffic control karta hai. Yehi wajah hai ki JS single-threaded hone ke bawajood asynchronous tasks smoothly perform kar sakti hai bina UI ko freeze kiye!
