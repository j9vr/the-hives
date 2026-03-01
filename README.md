<script>
// Always load owner
let defaultUsers = [
    { username: "j9vr", isMember: true, isOwner: true }
];

// Load saved users or create fresh
let users = JSON.parse(localStorage.getItem("hiveUsers"));
if (!users || users.length === 0) {
    users = defaultUsers;
    localStorage.setItem("hiveUsers", JSON.stringify(users));
}

let currentUser = null;
let isOwner = false;
let isRegister = false;

// Toggle login/register
function toggleAuth() {
    isRegister = !isRegister;
    document.getElementById("authTitle").textContent =
        isRegister ? "Create Account" : "The Hive System Login";
    document.getElementById("authBtn").textContent =
        isRegister ? "Register" : "Login";
    document.getElementById("passwordField").style.display = "none";
}

// Auth system (no password)
function authenticate() {
    const username = document.getElementById("authUsername").value.trim();
    if (!username) return alert("Enter a username");

    if (isRegister) {
        if (users.some(u => u.username === username))
            return alert("Username already exists");

        users.push({ username, isMember: false, isOwner: false });
        localStorage.setItem("hiveUsers", JSON.stringify(users));
        alert("Account created");
        toggleAuth();
        return;
    }

    // Login
    let user = users.find(u => u.username === username);
    if (!user) return alert("User not found");

    currentUser = user;
    isOwner = user.isOwner;

    document.getElementById("ownerPanelBtn").style.display =
        isOwner ? "block" : "none";

    document.getElementById("authScreen").style.display = "none";
    document.getElementById("hiveSystem").style.display = "block";
    refreshMembers();
}

// Logout
function logout() {
    currentUser = null;
    isOwner = false;
    document.getElementById("hiveSystem").style.display = "none";
    document.getElementById("authScreen").style.display = "flex";
}

// Toggle owner panel
function toggleOwnerPanel() {
    const panel = document.getElementById("ownerPanel");
    panel.style.display = panel.style.display === "block" ? "none" : "block";
}

// Refresh members
function refreshMembers() {
    const display = document.getElementById("membersListDisplay");
    display.innerHTML = "";

    users.filter(u => u.isMember).forEach(u => {
        const div = document.createElement("div");
        div.className = "member";
        div.textContent = `${u.username}${u.isOwner ? " (Owner)" : ""}`;
        display.appendChild(div);
    });

    if (!isOwner) return;

    const ownerList = document.getElementById("membersList");
    ownerList.innerHTML = "";

    users.forEach((u, i) => {
        const div = document.createElement("div");
        div.className = "member";
        div.innerHTML = `
            ${u.username} ${u.isOwner ? "(Owner)" : "(Member)"}
            <div class="memberControl">
                <button class="delete" onclick="removeMember(${i})">Remove</button>
            </div>`;
        ownerList.appendChild(div);
    });

    localStorage.setItem("hiveUsers", JSON.stringify(users));
}

// Grant member role
function grantMember() {
    const name = document.getElementById("grantUsername").value.trim();
    const user = users.find(u => u.username === name);
    if (!user) return alert("User not found");

    user.isMember = true;
    localStorage.setItem("hiveUsers", JSON.stringify(users));
    refreshMembers();
}

// Grant owner role
function grantOwner() {
    const name = document.getElementById("grantUsername").value.trim();
    const user = users.find(u => u.username === name);
    if (!user) return alert("User not found");

    user.isOwner = true;
    localStorage.setItem("hiveUsers", JSON.stringify(users));
    refreshMembers();
}

// Remove member
function removeMember(index) {
    if (!confirm("Remove user?")) return;
    users[index].isMember = false;
    users[index].isOwner = false;
    localStorage.setItem("hiveUsers", JSON.stringify(users));
    refreshMembers();
}
</script>name===username);
    if(!user) return alert("User not found");
    user.isOwner=true;
    localStorage.setItem("hiveUsers", JSON.stringify(users));
    document.getElementById("grantUsername").value="";
    refreshMembers();
}

// Remove member
function removeMember(index){
    const u = users[index];
    if(confirm(`Remove ${u.username} from members?`)){
        u.isMember=false;
        u.isOwner=false;
        refreshMembers();
    }
}
</script>
</body>
</html>
