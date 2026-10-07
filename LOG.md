# Debug log

Your notes. One entry per bug you fixed, using the template below.

This file is read as carefully as your code. A correct fix you cannot
explain counts for little; a bug you could not fix but investigated
honestly still counts for something.

Delete the example before you submit.



## Example — delete this

### CC-99 — "The cart total is wrong"

**Reproduced:** Added 2 dosas at Rs. 60 each. The cart showed
Rs. 119.99999 instead of Rs. 130. Happened every time, on any dish with
a price ending in .50.

**Cause:** The total was being added up with plain floating point and
never rounded, so 0.1 + 0.2 style errors showed up on screen. The
rounding helper existed but this one place was not using it.

**Fix:** Ran the total through the existing rounding helper instead of
adding a new one, so every price on screen goes through the same path.

**Checked:** Cart, checkout and the order screen all show Rs. 130 now.
Prices without decimals still show without a trailing .00.

**Time:** about 40 minutes, most of it working out that the cart and the
order screen round in different places.



## CC-0X — "<the complaint, in short>"

**Reproduced:**

**Cause:**

**Fix:**

**Checked:**

**Time:**



## Could not fix

For anything you investigated but did not solve. Say what you tried and
where you got to. This is worth marks — leaving it blank when you got
stuck is not.

### CC-0X — "<the complaint>"

**What I tried:**

**Where I got to:**

**What I would try next:**



## Extra credit

Anything not on the bug log: a problem you found yourself, a test you
wrote, or a fix you are unsure about. Same format, plus one line on how
you noticed it.



### CC-02: "Can't read anything in dark mode"

**Reproduced:**
- Opened the site on localhost
- Clicked dark mode toggle
- Food name and price of the food are not visible clearly

**Cause:**
- Opened DevTools using F12
- Went to the Elements tab and inspected the dish name/price
- Found they belong to class `.dish-body`, and in dark mode this class's text color isn't changing — stays same as light mode

**Fix:**
- In dark mode, set `.dish-body` text color to white (or light color) so it's visible against dark background 
- change the hard coded value to the variblr 
-  Replaced color: #2b2118 with color: var(--ink).

**Checked:** 
    - verified the harmode and ligh mode in the prices and name 

**Time:**

  - 20 -25 All (.dash-class - 5min code red karne me kaha kaha hai sab : 10 min )


### CC-05: "The category bar scrolls away on my phone"

**Reproduced:**
Mobile view mein menu ko scroll karne par category/filter bar gayab ho ja raha tha.

**Cause:**
Category/filter bar mein sticky positioning use nahi ki gayi thi.

**Fix:**
Mobile view ke liye category/filter bar mein sticky positioning add kar di.
- .wrap { position: sticky;
  top: 0;
  z-index: 1000;
}

**Checked:**
Mobile viewport par menu ko scroll karke check kiya aur verify kiya ki category/filter bar ab visible rehta hai.

**Time:** 15min
 
### CC-01 : "The search suggestions are behind everything"

**Reproduced:**
Search box mein kuch type karne par search suggestions doosre elements ke peeche chali ja rahi thi.

**Cause:**
`.search-wrap` ka `z-index` sirf `1` tha, jiski wajah se suggestions doosre elements ke peeche dikh rahi thi.

**Fix:**
`.search-wrap` ka `z-index` `1` se `1000` kar diya.

```css
.search-wrap {
  position: relative;
  z-index: 1000;
}
```

**Checked:**
Search karke check kiya. Ab suggestions doosre elements ke upar properly dikh rahi hain aur click bhi ho rahi hain.

**Time:**
- 10min

### CC-03 : "The menu has horizontal overflow on mobile"

**Reproduced:**
Mobile screen par menu/content screen ki width se bahar ja raha tha.

**Cause:**
Mobile layout mein kuch elements apni available width se zyada space le rahe the.

**Fix:**
Mobile layout mein `.name-btn` ki width ko `100%` kiya aur `.dish-card` mein `min-width: 0` add kiya.

**Checked:**
Mobile view mein 375px width par test kiya. Ab menu screen ke andar properly fit ho raha hai aur unnecessary horizontal page scrolling nahi hai.

**Time:** 15 min


### CC-04: "The buttons don't work on my tablet"

**Reproduced:**
DevTools me width 800px rakhi. Add to Cart aur star button normal dikhe
par click karne pe kuch nahi hua. 1000px aur 500px pe sab theek tha.
Button ko inspect kiya to upar koi aur element dikha.

**Cause:**
style.css me `@media (min-width: 761px) and (max-width: 900px)` block
hai. Usme `.dish-card::after` (neeche 58px ki patti) aur `.img-wrap::after`
(top-right 52px ka box) transparent `position: absolute` layers bane the,
jo Add to Cart aur star button ke upar baithke click rok rahe the. Isi
wajah se bug sirf tablet width pe aata tha.

**Fix:**
Dono `::after` rules me `pointer-events: none;` add kiya, taaki click
neeche button tak pahunche. Block hataya nahi kyunki `.blocked` ka dim
effect rakhna tha.

**Checked:**
800px pe dono buttons chal rahe hain. 1000px aur 375px pe layout pehle
jaisa hai. Blocked dish ka Add button abhi bhi disabled hai.

**Time:** 20 min



### CC-10: "Sorting by price is backwards"

**Reproduced:**
Menu me "Price: low to high" chuna to sabse mehnga dish pehle aaya, aur
"high to low" me sasta pehle. `/api/menu?sort=price-asc` call karne pe
bhi prices ulti aayi, yaani bug server me tha.

**Cause:**
`backend/logic/search.js` ke `SORTERS` me `price-asc` ka comparator
`b.price - a.price` tha aur `price-desc` ka `a.price - b.price`. Dono
ulte likhe the.

**Fix:**
`price-asc` ko `a.price - b.price` aur `price-desc` ko `b.price - a.price`
kiya. Server me isliye badla kyunki sorting wahi decide hoti hai,
frontend sirf dikhata hai.

**Checked:**
`?sort=price-asc` me prices chhoti se badi aur `price-desc` me badi se
chhoti aa rahi hain. Baaki sorts (rating, name) waise hi chal rahe hain.

**Time:** 10min

### CC-09: "The menu shows more dishes than it should"

**Reproduced:**
Menu page kholne par saari dishes ek saath aa gayi. `/api/menu?limit=5`
call karne pe `dishes` me 5 ki jagah poori list (37) aayi, jabki `total`
aur `hasMore` sahi the.

**Cause:**
`backend/logic/search.js` ke `paginate()` me page ka slice `items` me
ban raha tha, par return me `items: list` likha tha, yaani poori list.
Isliye slice kabhi use hi nahi hua.

**Fix:**
Return me `items: list` ko `items` kiya. Server me isliye badla kyunki
pagination wahi karta hai, frontend sirf jo mile wo dikhata hai.

**Checked:**
`?limit=5` me 5 dishes aayi, `total` 37 aur `hasMore: true`. `page=2` me
agli 5 dishes aayi. Menu page par scroll karne pe aur dishes load ho rahi
hain.

**Time:** 10min

### CC-06: "I ordered more than they had"

**Reproduced:**
Menu.json me ek dish ka stock dekha, phir `/api/orders` pe usse zyada
qty bhejke order kiya. Order ban gaya (201) aur stock negative ho gaya.

**Cause:**
`backend/logic/validation.js` ke `validateLine()` me sirf stock `<= 0`
(sold out) check hota tha, qty stock se zyada hai ya nahi ye nahi.
`reserveStock()` bhi bina check kiye `dish.stock -= item.qty` kar deta
tha, jabki uska comment kehta tha ki wo har line ko re-check karta hai.

**Fix:**
`validateLine()` me `qty > stock` par "only N left" error add kiya, aur
`reserveStock()` me subtract se pehle wahi check lagaya, taaki stock
kabhi negative na ho. Server me isliye badla kyunki stock ka sach wahi
decide karta hai, frontend sirf dikhata hai.

**Checked:**
Stock se zyada qty bhejne par 400 "only N left" aaya. Stock ke barabar
qty par order ho gaya (201). Test ke baad `backend/data/` reset kiya.

**Time:** 25 min 


### CC-07: "Cancelling makes it worse"

**Reproduced:**
Ek dish ka order place kiya (stock 6 se 4 hua), phir order cancel kiya.
Stock 6 par wapas aane ki jagah 2 ho gaya.

**Cause:**
`backend/logic/validation.js` ke `releaseStock()` me
`stock: dish.stock - line.qty` likha tha. Cancel par stock wapas add
hona chahiye tha, par minus hone se ek baar aur ghat jata tha.

**Fix:**
`-` ko `+` kiya. Logic yahin badla kyunki stock ka hisaab isi function
se hota hai, route me sirf isko call kiya jata hai.

**Checked:**
Stock 6, order ke baad 4, cancel ke baad wapas 6. Test ke baad
`backend/data/` reset kiya.

**Time:** 15min


### CC-08: "An old coupon still works"

**Reproduced:**
Cart me Rs. 100 se upar ke dishes daalke FRESHERS24 lagaya. Rs. 30 ka
discount mil gaya, jabki coupons.json me uska `expiresAt` 2024-09-30 hai.

**Cause:**
`backend/logic/pricing.js` ke `applyCoupon()` me `expiresAt` check hi
nahi tha. Expiry sirf `GET /api/coupons` ki list me filter hoti hai, jo
coupon ko dikhne se rokti hai, lagne se nahi. Code manually type karne
par validation pass ho jati thi.

**Fix:**
`applyCoupon()` me coupon milne ke baad `expiresAt <= now` check add
kiya, jo "has expired" error deta hai. Yahin isliye kyunki quote aur
order dono isi function se guzarte hain, to ek jagah fix se dono raaste
band ho gaye.

**Checked:**
FRESHERS24 ab "has expired" deta hai aur discount 0 hai. BYTE10 abhi bhi
chal raha hai.

**Time:** 22 min