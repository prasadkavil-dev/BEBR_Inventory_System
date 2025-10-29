// === BEBR Inventory v2.8 ===
// SharePoint-ready with demo data

const configKey = "bebrConfig";
const defaultConfig = {
  siteUrl: "https://<company>.sharepoint.com/sites/spcad-pu98ryp0c2esb",
  inventoryList: "BEBR-Inventory",
  usersList: "BEBR-Users",
  theme: "dark",
  exportType: "csv"
};
let appConfig = JSON.parse(localStorage.getItem(configKey)) || defaultConfig;

// Demo data
const demoProducts = [
  {id:1,name:"Keyboard",barcode:"1111",qty:50,expiry:"-",batch:"A1"},
  {id:2,name:"Mouse",barcode:"2222",qty:35,expiry:"-",batch:"A2"},
  {id:3,name:"Monitor",barcode:"3333",qty:20,expiry:"-",batch:"A3"},
];
const demoUsers = [
  {name:"Alice",role:"Admin"},
  {name:"Bob",role:"Manager"},
  {name:"Carol",role:"Editor"},
  {name:"Dave",role:"Viewer"}
];
const demoReports = [
  {date:"2025-10-01",action:"Inward",user:"Carol",item:"Keyboard",qty:10},
  {date:"2025-10-02",action:"Outward",user:"Carol",item:"Mouse",qty:5},
  {date:"2025-10-03",action:"Edit",user:"Bob",item:"Monitor",qty:2},
];

// === Sidebar ===
function loadSidebar(){
  const sidebar=document.getElementById("sidebar");
  if(!sidebar) return;
  sidebar.innerHTML=`
  <h2>BEBR</h2>
  <a href="index.html">Dashboard</a>
  <a href="products.html">Products</a>
  <a href="inward.html">Inward</a>
  <a href="outward.html">Outward</a>
  <a href="closing.html">Closing</a>
  <a href="analytics.html">Analytics</a>
  <a href="reports.html">Reports</a>
  <a href="users.html">Users</a>
  <a href="settings.html" id="settingsLink">Settings</a>
  `;
}
loadSidebar();

// === Page-specific logic ===
document.addEventListener("DOMContentLoaded",()=>{
  const page=location.pathname.split("/").pop();
  if(page==="index.html") loadDashboard();
  if(page==="products.html") loadProducts();
  if(page==="reports.html") loadReports();
  if(page==="users.html") loadUsers();
  if(page==="settings.html") loadSettings();
});

function loadDashboard(){
  const area=document.getElementById("summaryCards");
  if(!area) return;
  const total=demoProducts.reduce((a,b)=>a+b.qty,0);
  area.innerHTML=`
    <div class="card">Total Products: ${demoProducts.length}</div>
    <div class="card">Total Quantity: ${total}</div>
    <div class="card">Transactions: ${demoReports.length}</div>
  `;
}
function loadProducts(){
  const t=document.getElementById("productTable");
  if(!t) return;
  t.innerHTML="<tr><th>Name</th><th>Barcode</th><th>Qty</th><th>Batch</th></tr>"+
    demoProducts.map(p=>`<tr><td>${p.name}</td><td>${p.barcode}</td><td>${p.qty}</td><td>${p.batch}</td></tr>`).join("");
}
function loadReports(){
  const t=document.getElementById("reportTable");
  if(!t) return;
  t.innerHTML="<tr><th>Date</th><th>Action</th><th>User</th><th>Item</th><th>Qty</th></tr>"+
    demoReports.map(r=>`<tr><td>${r.date}</td><td>${r.action}</td><td>${r.user}</td><td>${r.item}</td><td>${r.qty}</td></tr>`).join("");
}
function loadUsers(){
  const t=document.getElementById("userTable");
  if(!t) return;
  t.innerHTML="<tr><th>Name</th><th>Role</th></tr>"+
    demoUsers.map(u=>`<tr><td>${u.name}</td><td>${u.role}</td></tr>`).join("");
}
function loadSettings(){
  const f=document.getElementById("settingsForm");
  if(!f) return;
  f.innerHTML=`
    <label>Site URL <input id="siteUrl" value="${appConfig.siteUrl}"></label>
    <label>Inventory List <input id="invList" value="${appConfig.inventoryList}"></label>
    <label>User List <input id="usrList" value="${appConfig.usersList}"></label>
    <button id="saveCfg">Save</button>
  `;
  document.getElementById("saveCfg").onclick=()=>{
    appConfig.siteUrl=document.getElementById("siteUrl").value;
    appConfig.inventoryList=document.getElementById("invList").value;
    appConfig.usersList=document.getElementById("usrList").value;
    localStorage.setItem(configKey,JSON.stringify(appConfig));
    alert("Saved!");
  };
}