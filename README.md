<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🎨 Art Gallery App</title>
<style>
html,body{margin:0;padding:0;height:100%;font-family:'Segoe UI',sans-serif;display:flex;flex-direction:column;}
body{background:linear-gradient(135deg,#d9a066,#c6894f,#b87333);background-size:400% 400%;animation:caramelFlow 25s ease infinite;color:#3b2f2f;}
@keyframes caramelFlow{0%{background-position:0% 50%}50%{background-position:100% 50%}100%{background-position:0% 50%}}
.navbar{display:flex;align-items:center;justify-content:space-between;background:rgba(80,50,30,0.9);padding:0.8rem 1rem;position:sticky;top:0;z-index:1000;backdrop-filter:blur(5px);color:#fff3e6;}
.logo{font-size:1.9rem;font-weight:bold;color:#fff1d0;letter-spacing:1px;}
.search-box{flex:1;margin:0 1rem;}
.search-box input{width:100%;padding:11px;border:none;border-radius:10px;font-size:1rem;box-shadow:0 0 6px rgba(0,0,0,0.2);}
.profile-btn{background:#fff1d0;color:#4b2e2e;border:none;border-radius:50%;width:44px;height:44px;font-size:1.4rem;cursor:pointer;box-shadow:0 2px 5px rgba(0,0,0,0.3);}
.category-sort-container{display:flex;justify-content:space-between;align-items:center;background:rgba(255,248,240,0.85);padding:12px;gap:10px;flex-wrap:wrap;box-shadow:0 1px 4px rgba(0,0,0,0.2);}
.categories{display:flex;overflow-x:auto;gap:12px;}
.categories button{background:#b87333;color:white;border:none;border-radius:20px;padding:10px 18px;cursor:pointer;font-size:1.05rem;white-space:nowrap;transition: background 0.3s;}
.categories button:hover{background:#d29a6c;}
.sort-dropdown{font-size:1rem;padding:6px;border-radius:6px;border:1px solid #aaa;background:#fff8f0;}
main#gallery{flex:1;display:grid;grid-template-columns:repeat(auto-fit,minmax(320px,1fr));gap:30px;padding:30px;}
.card{background: rgba(255,248,240,0.9); border-radius:16px; box-shadow:0 7px 18px rgba(0,0,0,0.25); text-align:center; transition:transform 0.3s,box-shadow 0.3s; display:flex;flex-direction:column;overflow:hidden;backdrop-filter:blur(4px);}
.card:hover{transform:translateY(-8px);box-shadow:0 10px 22px rgba(0,0,0,0.35);}
.card img{width:100%;height:420px;object-fit:contain;transition:transform 0.4s ease, filter 0.4s ease;background:#fff8f0;}
.card:hover img{transform:scale(1.08);filter:brightness(1.05);}
.card h3{margin:12px 0 6px;font-size:1.7rem;color:#6d4c41;}
.card p{font-size:1.25rem;margin:0;color:#4b2e2e;}
.btn-row{display:flex;justify-content:space-around;margin:14px 0;}
.btn{background:#b87333;color:white;padding:12px 24px;border:none;border-radius:8px;font-size:1.2rem;cursor:pointer;transition: background 0.3s;}
.btn:hover{background:#d29a6c;}
.wishlist-btn{background:none;border:none;font-size:2rem;cursor:pointer;color:gray;transition: color 0.3s;}
.wishlist-btn.active{color:red;}
.drawer{position:fixed;top:0;right:-300px;width:300px;height:100%;background:#fff8f0;box-shadow:-2px 0 6px rgba(0,0,0,0.3);padding:20px;transition:right 0.3s ease-in-out;z-index:2000;display:none;}
.drawer.open{display:block;right:0;}
.drawer button{display:block;margin:12px 0;background:#b87333;color:white;border:none;padding:12px;border-radius:6px;cursor:pointer;width:100%;font-size:1rem;}
footer{background: rgba(80,50,30,0.9);color:#fff1d0;text-align:center;padding:80px 12px;font-size:1.1rem;margin-top:20px;}
.modal{display:none;position:fixed;z-index:3000;left:0;top:0;width:100%;height:100%;overflow:auto;background: rgba(0,0,0,0.8);justify-content:center;align-items:center;flex-direction:column;text-align:center;color:white;padding:20px;}
.modal-content{max-width:80%;max-height:70%;border-radius:12px;margin-bottom:20px;}
.close-btn{position:absolute;top:20px;right:30px;font-size:3rem;color:white;cursor:pointer;font-weight:bold;}
#uploadForm{display:none;position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:#fff8f0;padding:25px;border-radius:16px;box-shadow:0 0 20px rgba(0,0,0,0.4);z-index:4000;}
#uploadForm input,#uploadForm textarea{width:100%;margin:10px 0;padding:10px;border-radius:8px;border:1px solid #aaa;font-size:1rem;}
#loginForm{display:flex;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.85);justify-content:center;align-items:center;z-index:5000;flex-direction:column;color:#fff;}
#loginForm h2{margin-bottom:20px;}
#loginForm input{margin:8px 0;padding:12px;border-radius:8px;border:none;width:250px;font-size:1rem;}
#loginForm button{margin:8px 0;padding:12px 24px;border:none;border-radius:8px;cursor:pointer;font-size:1rem;background:#b87333;color:white;transition: background 0.3s;}
#loginForm button:hover{background:#d29a6c;}
#loginForm small{cursor:pointer;color:#d29a6c;margin-top:6px;display:block;}
</style>
</head>
<body>

<!-- Login -->
<div id="loginForm">
<h2 id="formTitle">Login</h2>
<input type="text" id="username" placeholder="Username">
<input type="password" id="password" placeholder="Password">
<button onclick="submitLogin()">Login</button>
<small onclick="toggleForm()">Don't have an account? Signup</small>
</div>

<!-- Navbar -->
<div class="navbar" style="display:none;">
<div class="logo">🎨 ArtGallery</div>
<div class="search-box">
<input type="text" id="search" placeholder="Search artworks or artists...">
</div>
<button class="profile-btn" onclick="toggleDrawer()">👤</button>
</div>

<!-- Category/Sort -->
<div class="category-sort-container" style="display:none;">
<div class="categories">
<button onclick="filterCategory('all')">All</button>
<button onclick="filterCategory('Portrait')">Portrait</button>
<button onclick="filterCategory('Abstract')">Abstract</button>
<button onclick="filterCategory('Modern')">Modern</button>
<button onclick="filterCategory('Nature')">Nature</button>
</div>
<button class="btn" onclick="openUploadForm()">➕ Upload Artwork</button>
<select id="sort" class="sort-dropdown" onchange="sortArtworks()">
<option value="default">Sort By</option>
<option value="title">Title (A-Z)</option>
<option value="artist">Artist (A-Z)</option>
<option value="recent">Recently Added</option>
</select>
</div>

<main id="gallery"></main>

<!-- Drawer -->
<div class="drawer" id="settingsDrawer">
<h2>⚙ Settings</h2>
<button onclick="toggleDarkMode()">🌙 Toggle Dark Mode</button>
<button onclick="changeLanguage()">🌐 Change Language</button>
<button onclick="increaseFont()">🔼 Bigger Font</button>
<button onclick="decreaseFont()">🔽 Smaller Font</button>
<button onclick="showWishlist()">❤ View Wishlist</button>
<button onclick="logout()">🚪 Logout</button>
<button onclick="toggleDrawer()">❌ Close</button>
</div>

<footer>&copy; 2025 ArtGallery | Your Favorite Art ❤</footer>

<!-- Modal -->
<div class="modal" id="artModal">
<span class="close-btn" onclick="closeModal()">&times;</span>
<img class="modal-content" id="modalImage">
<p id="modalDesc"></p>
</div>

<!-- Upload -->
<div id="uploadForm">
<h2>Upload Your Artwork</h2>
<input type="text" id="uploadTitle" placeholder="Artwork Title">
<input type="text" id="uploadArtist" placeholder="Artist Name">
<input type="text" id="uploadCategory" placeholder="Category">
<input type="text" id="uploadImage" placeholder="Image URL">
<textarea id="uploadDesc" placeholder="Add a short description"></textarea>
<button class="btn" onclick="submitArtwork()">Upload</button>
<button class="btn" style="background:#777" onclick="closeUploadForm()">Cancel</button>
</div>

<script>
// ===== Login/Signup =====
let isLogin = true;
function toggleForm(){
isLogin = !isLogin;
document.getElementById("formTitle").innerText = isLogin?"Login":"Signup";
document.querySelector("#loginForm button").innerText = isLogin?"Login":"Signup";
document.querySelector("#loginForm small").innerText = isLogin?"Don't have an account? Signup":"Already have an account? Login";
}
function submitLogin(){
const username = document.getElementById("username").value.trim();
const password = document.getElementById("password").value.trim();
if(!username||!password){ alert("⚠ Enter username and password!"); return; }
let users = JSON.parse(localStorage.getItem("users")||"[]");
if(isLogin){
const user = users.find(u=>u.username===username && u.password===password);
if(user){ alert(✅ Welcome ${username}); document.getElementById("loginForm").style.display="none"; showGallery(); }
else alert("❌ Invalid credentials!");
} else {
if(users.some(u=>u.username===username)){ alert("❌ Username exists!"); return; }
users.push({username,password}); localStorage.setItem("users",JSON.stringify(users));
alert("✅ Signup successful! You can login now."); toggleForm();
}
}

// ===== Artworks =====
let artworks=[
{ title:"Mona Lisa", artist:"Leonardo da Vinci", category:"Portrait", description:"Famous portrait with enigmatic smile.", added:1, image:"https://upload.wikimedia.org/wikipedia/commons/6/6a/Mona_Lisa.jpg" },
{ title:"The Scream", artist:"Edvard Munch", category:"Abstract", description:"Expressionist masterpiece symbolizing anxiety.", added:2, image:"https://upload.wikimedia.org/wikipedia/commons/f/f4/The_Scream.jpg" },
{ title:"Girl with a Pearl Earring", artist:"Johannes Vermeer", category:"Portrait", description:"The 'Mona Lisa of the North'.", added:3, image:"https://upload.wikimedia.org/wikipedia/commons/d/d7/Meisje_met_de_parel.jpg" },
{ title:"Guernica", artist:"Pablo Picasso", category:"Modern", description:"Powerful political statement against war.", added:4, image:"https://upload.wikimedia.org/wikipedia/en/7/74/PicassoGuernica.jpg" },
{ title:"The Persistence of Memory", artist:"Salvador Dalí", category:"Abstract", description:"Melting clocks representing time fluidity.", added:5, image:"https://upload.wikimedia.org/wikipedia/en/d/dd/The_Persistence_of_Memory.jpg" }
];
let wishlist=[];
const gallery=document.getElementById("gallery");
const searchBox=document.getElementById("search");
const drawer=document.getElementById("settingsDrawer");
const modal=document.getElementById("artModal");
const modalImage=document.getElementById("modalImage");
const modalDesc=document.getElementById("modalDesc");
const uploadForm=document.getElementById("uploadForm");

// ===== Display =====
function showGallery(){
document.querySelector(".navbar").style.display="flex";
document.querySelector(".category-sort-container").style.display="flex";
displayArtworks(artworks);
}
function displayArtworks(list){
gallery.innerHTML="";
if(list.length===0){ gallery.innerHTML='<p style="text-align:center;font-size:1.3rem;font-weight:bold;color:#6d4c41;">😔 No artworks found.</p>'; return; }
list.forEach(art=>{
const isInWishlist=wishlist.includes(art.title);
const card=document.createElement("div");
card.classList.add("card");
card.innerHTML=`
<img src="${art.image}" alt="${art.title}">
<h3>${art.title}</h3>
<p><b>${art.artist}</b></p>
<div class="btn-row">
<button class="btn" onclick="viewArt('${art.image}','${art.description}')">View</button>
<button class="btn" onclick="buyArt('${art.title}')">Buy</button>
<button class="wishlist-btn ${isInWishlist?'active':''}" onclick="toggleWishlist('${art.title}')">❤</button>
</div>
`;
gallery.appendChild(card);
});
}

// ===== SEARCH/FILTER/SORT =====
searchBox.addEventListener("input", ()=>{
const q=searchBox.value.toLowerCase();
displayArtworks(artworks.filter(a=>a.title.toLowerCase().includes(q)||a.artist.toLowerCase().includes(q)));
});
function filterCategory(cat){ cat==="all"?displayArtworks(artworks):displayArtworks(artworks.filter(a=>a.category===cat)); }
function sortArtworks(){ const s=document.getElementById("sort").value; let arr=[...artworks]; if(s==="title")arr.sort((a,b)=>a.title.localeCompare(b.title)); else if(s==="artist")arr.sort((a,b)=>a.artist.localeCompare(b.artist)); else if(s==="recent")arr.sort((a,b)=>b.added-a.added); displayArtworks(arr); }

// ===== Drawer =====
function toggleDrawer(){drawer.classList.toggle("open");}
function toggleDarkMode(){ document.body.style.background=document.body.style.background==="black"?"linear-gradient(135deg,#d9a066,#c6894f,#b87333)":"black"; document.body.style.color=document.body.style.color==="white"?"#3b2f2f":"white"; }
function increaseFont(){document.body.style.fontSize="larger";}
function decreaseFont(){document.body.style.fontSize="smaller";}
function changeLanguage(){ const lang=prompt("Enter Language (English,Hindi,Telugu):"); alert(Language changed to ${lang} (coming soon));}
function toggleWishlist(title){ wishlist.includes(title)?wishlist=wishlist.filter(t=>t!==title):wishlist.push(title); displayArtworks(artworks);}
function showWishlist(){displayArtworks(artworks.filter(a=>wishlist.includes(a.title))); toggleDrawer();}

// ===== Modal =====
function viewArt(src,desc){ modal.style.display="flex"; modalImage.src=src; modalDesc.innerText=desc; }
function closeModal(){ modal.style.display="none"; }
window.onclick=e=>{ if(e.target===modal) modal.style.display="none"; }

// ===== Buy =====
function buyArt(title){ const price=(Math.random()*500+50).toFixed(2); alert(🖼 "${title}"\nPrice: $${price}); }

// ===== Upload =====
function openUploadForm(){ uploadForm.style.display="block"; }
function closeUploadForm(){ uploadForm.style.display="none"; }
function submitArtwork(){
const t=document.getElementById("uploadTitle").value;
const a=document.getElementById("uploadArtist").value;
const c=document.getElementById("uploadCategory").value;
const i=document.getElementById("uploadImage").value;
const d=document.getElementById("uploadDesc").value;
if(t && a && c && i){ artworks.push({title:t,artist:a,category:c,image:i,description:d,added:artworks.length+1}); displayArtworks(artworks); closeUploadForm(); alert("✅ Artwork uploaded!"); }
else alert("⚠ Please fill all fields!");
}

// ===== Logout =====
function logout(){ location.reload(); }

</script>
</body>
</html>
