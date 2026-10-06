
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Shahabuddin Crockery Store</title>
<script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="max-w-[480px] mx-auto bg-[#f4f7f9] pb-20">
<div class="bg-gradient-to-r from-[#1a3a4a] to-[#2a9db0] text-white p-5 text-center rounded-b-[20px] m-2 mt-0">
<h1 class="text-[26px] font-extrabold">Shahabuddin Crockery Store</h1>
<p class="text-[13px]">Shop No 41 Unit No 8 Jama Cloth Market Latifabad Hyderabad</p>
<p class="text-[15px] font-bold mt-1">Phone: 03183162760 / 03133362512</p>
</div>

<div class="bg-white mx-3 mt-4 rounded-2xl shadow-sm border p-5">
<h2 class="font-bold text-right text-[18px] border-b pb-2 mb-4">Add new items and stock</h2>
<label class="block text-right font-bold text-sm mb-1">:Item Name</label>
<input id="itemName" placeholder="Write the name." class="w-full text-right border rounded-lg px-4 py-3 mb-4">
<label class="block text-right font-bold text-sm mb-1">:Price (Rs.)</label>
<input id="itemPrice" type="number" placeholder="Write a price" class="w-full text-right border rounded-lg px-4 py-3 mb-4">
<label class="block text-right font-bold text-sm mb-1">:Stock Quantity</label>
<input id="itemQty" type="number" placeholder="Write the number." class="w-full text-right border rounded-lg px-4 py-3 mb-4">
<button onclick="addStock()" class="w-full bg-[#0fb26a] text-white font-bold py-3.5 rounded-lg">Add to stock</button>
<div id="stockList" class="mt-5"></div>
</div>

<div class="bg-white mx-3 mt-4 rounded-2xl shadow-sm border p-5">
<h2 class="font-bold text-right text-[18px] border-b pb-2 mb-4">Billing and Sales (Cart System)</h2>
<label class="block text-right font-bold text-sm mb-1">:Customer Name (Optional)</label>
<input id="custName" placeholder="Write the customer's name." class="w-full text-right border rounded-lg px-4 py-3 mb-4">
<label class="block text-right font-bold text-sm mb-1">:Customer phone number</label>
<input id="custPhone" placeholder="Write down the phone number" class="w-full text-right border rounded-lg px-4 py-3 mb-4">
<label class="block text-right font-bold text-sm mb-1">:Select Item</label>
<select id="selectItem" class="w-full text-right border rounded-lg px-4 py-3 mb-3"></select>
<div class="flex gap-2 mb-3">
<input id="sellQty" type="number" value="1" class="w-20 border rounded-lg py-3 text-center">
<button onclick="addToCart()" class="flex-1 bg-[#1a3a4a] text-white font-bold py-3 rounded-lg">Add to Cart</button>
</div>
<div id="cart" class="border rounded-lg divide-y"></div>
<div class="flex justify-between font-bold text-lg mt-3"><span>Total:</span><span>Rs. <span id="cartTotal">0</span></span></div>
<button onclick="checkout()" class="w-full mt-4 bg-[#0fb26a] text-white font-bold py-3.5 rounded-lg">Generate Bill & Save Sale</button>
</div>

<script>
let stock=JSON.parse(localStorage.getItem('shahab_stock')||'[]'); let cart=[];
function save(){localStorage.setItem('shahab_stock',JSON.stringify(stock));render();}
function addStock(){let n=itemName.value.trim(),p=parseInt(itemPrice.value),q=parseInt(itemQty.value); if(!n||!p||!q) return alert('Fill all'); let ex=stock.find(s=>s.name.toLowerCase()==n.toLowerCase()); if(ex){ex.qty+=q;ex.price=p;} else stock.push({name:n,price:p,qty:q}); itemName.value='';itemPrice.value='';itemQty.value=''; save();}
function render(){stockList.innerHTML=''; selectItem.innerHTML='<option value="">-- Select --</option>'; stock.forEach((it,i)=>{stockList.innerHTML+=`<div class="flex justify-between bg-gray-50 border rounded-lg px-3 py-2 mb-2 text-sm"><span class="font-bold">${it.name}</span><span>Rs.${it.price} | Qty:${it.qty}</span><button onclick="stock.splice(${i},1);save()" class="text-red-500">X</button></div>`; selectItem.innerHTML+=`<option value="${i}">${it.name} - Rs.${it.price} (Stock ${it.qty})</option>`;}); renderCart();}
function addToCart(){let idx=selectItem.value,q=parseInt(sellQty.value); if(idx===''||!q) return; if(stock[idx].qty<q) return alert('Stock kam hai!'); cart.push({name:stock[idx].name,price:stock[idx].price,qty:q,idx}); renderCart();}
function renderCart(){cart.innerHTML=''; let tot=0; cart.forEach((it,i)=>{tot+=it.price*it.qty; cart.innerHTML+=`<div class="flex justify-between p-2 text-sm"><span>${it.name} x${it.qty}</span><span>Rs.${it.price*it.qty} <button onclick="cart.splice(${i},1);renderCart()" class="text-red-500">x</button></span></div>`;}); cartTotal.innerText=tot;}
function checkout(){if(cart.length==0) return alert('Cart khali'); cart.forEach(it=>{stock[it.idx].qty-=it.qty;}); cart=[]; save(); alert('Bill save ho gaya!');}
render();
</script>
</body>
</html>
