<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>The Hive System | DXRP</title>
<style>
body { margin:0; font-family:Arial, sans-serif; background:#000; color:#fff; }
#loginScreen { display:flex; flex-direction:column; align-items:center; justify-content:center; height:100vh; }
input, select { width:250px; padding:10px; margin:8px 0; border:none; border-radius:5px; background:#111; color:#00ffcc; text-align:center; font-size:16px; }
button { width:270px; padding:10px; margin-top:10px; border:none; border-radius:5px; background:#00ffcc; color:#000; font-weight:bold; cursor:pointer; }
button:hover { background:#00bfa5; }
#hiveSystem { display:none; }
.nav { display:flex; justify-content:space-between; padding:15px 30px; background:#111; border-bottom:2px solid #00ffcc; }
.logo { font-size:25px; font-weight:bold; color:#00ffcc; }
.content { padding:20px 40px; }
.section { background:#111; border-left:4px solid #00ffcc; border-radius:5px; margin-bottom:20px; padding:20px; }
.section h2 { color:#00ffcc; margin-bottom:10px; }
.member { background:#222; padding:10px; margin:5px 0; border-left:3px solid #00ffcc; display:flex; justify-content:space-between; align-items:center; }
.ownerBtn { background:#ffaa00; color:#000; margin-top:10px; width:auto; padding:5px 10px; }
.ownerPanel { display:none; background:#111; border-left:4px solid #ffaa00; padding:15px; margin-top:20px; border-radius:5px; }
.ownerPanel input, .ownerPanel select { width:200px; margin-right:5px; }
.memberControl { display:flex; gap:5px; }
.memberControl button { padding:2px 5px; border:none; border-radius:3px; cursor:pointer; }
.memberControl button.edit { background:#ffaa00; color:#000; }
.memberControl button.delete { background:#ff4444; color:#fff; }
</style>
</head>
<body>

<!-- LOGIN -->
<div id="loginScreen">
<h1>The Hive System Login</h1>
<p>Enter your DXRP credentials</p>
<input type="text" id="username" placeholder="Username">
<input type="password" id="password" placeholder="Password">
<button onclick="login()">Login</button>
<p style="margin-top:10px; color:#00ffcc;">Demo: Username: hive | Password: dxrp (Owner login)</p>
</div>

<!-- HIVE SYSTEM -->
<div id="hiveSystem">
<div class="nav">
    <div class="logo">THE HIVE SYSTEM</div>
    <div><button class="ownerBtn" onclick="logout()">Logout</button></div>
</div>

<div class="content">
<h1>Welcome to The Hive DXRP Dashboard</h1>
<p>Faction control, member tracking, and DXRP operations.</p>

<!-- MEMBERS SECTION -->
<div class="section">
<h2>Members</h2>
<div id="membersListDisplay"></div>

<button class="ownerBtn" onclick="toggleOwnerPanel()">Owner Panel</button>

<div class="ownerPanel" id="ownerPanel">
<h3>Manage Members</h3>
<input id="newMemberName" placeholder="New Member Name">
<select id="newMemberRole">
    <option value="Leader">Leader</option>
    <option value="Co-Leader">Co-Leader</option>
    <option value="Soldier">Soldier</option>
    <option value="Recruit">Recruit</option>
</select>
<button onclick="addMember()">Add Member</button>

<div id="membersList"></div>
</div>
</div>

<!-- FACTION INFO -->
<div class="section">
<h2>Faction Info</h2>
<p>Strength: Elite</p>
<p>Status: Active</p>
<p>Territories Controlled: Downtown, East Side</p>
</div>

<div class="section">
<h2>Operations</h2>
<p>• Raid planning in progress</p>
<p>• Surveillance active</p>
<p>• DXRP alerts monitored</p>
</div>
</div>

<script>
let isOwner=false;
let members=[{name:"j9vr",role:"Co-Leader"},{name:"MemberA",role:"Soldier"}];

function login(){
    const user=document.getElementById("username").value;
    const pass=document.getElementById("password").value;
    if(user==="hive" && pass==="dxrp"){
        isOwner=true;
        document.getElementById("loginScreen").style.display="none";
        document.getElementById("hiveSystem").style.display="block";
        refreshMembers();
    } else alert("Incorrect username or password.");
}

function logout(){
    document.getElementById("hiveSystem").style.display="none";
    document.getElementById("loginScreen").style.display="flex";
}

// Toggle Owner Panel
function toggleOwnerPanel(){
    const panel=document.getElementById("ownerPanel");
    panel.style.display=(panel.style.display==="block"?"none":"block");
}

// Refresh member list in both display and owner panel
function refreshMembers(){
    const displayDiv=document.getElementById("membersListDisplay");
    displayDiv.innerHTML="";
    members.forEach(m=>{
        const div=document.createElement("div");
        div.className="member";
        div.textContent=`${m.name} (${m.role})`;
        displayDiv.appendChild(div);
    });

    const ownerDiv=document.getElementById("membersList");
    ownerDiv.innerHTML="";
    members.forEach((m,index)=>{
        const div=document.createElement("div");
        div.className="member";
        div.innerHTML=`${m.name} (${m.role}) <div class="memberControl">
            <button class="edit" onclick="editMember(${index})">Edit</button>
            <button class="delete" onclick="removeMember(${index})">Remove</button>
        </div>`;
        ownerDiv.appendChild(div);
    });
}

// Add new member
function addMember(){
    const name=document.getElementById("newMemberName").value.trim();
    const role=document.getElementById("newMemberRole").value;
    if(name===""){ alert("Enter a name"); return; }
    members.push({name:name, role:role});
    document.getElementById("newMemberName").value="";
    refreshMembers();
}

// Edit member
function editMember(index){
    const m=members[index];
    const newName=prompt("Edit member name:", m.name);
    if(!newName) return;
    const newRole=prompt("Edit member role (Leader, Co-Leader, Soldier, Recruit):", m.role);
    if(!newRole) return;
    members[index]={name:newName,role:newRole};
    refreshMembers();
}

// Remove member
function removeMember(index){
    if(confirm("Remove this member?")){
        members.splice(index,1);
        refreshMembers();
    }
}
</script>
</body>
<div class="logo">The Hive System</div>
</html>
