# loanpilot1.io
<!DOCTYPE html>
<html lang="en">
<head>
  <meta name="fast2sms" content="2dt6poTz5kPrrqRGNCYek6BH2nF1Tmku">
  <meta charset="UTF-8">
  <title>User Dashboard</title>
  <style>
    body { font-family: sans-serif; background: #f0f0f0; margin: 0; padding: 0; }
    .navbar { background: #343a40; color: white; padding: 15px 20px; display: flex; justify-content: space-between; }
    .navbar a { color: white; text-decoration: none; margin-right: 15px; }
    .container { padding: 20px; }
    .profile-card { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 0 8px rgba(0,0,0,0.1); max-width: 500px; margin: auto; }
  </style>
</head>
<body>

  <div class="navbar">
    <div>
      <strong>LoanPilot Dashboard</strong>
    </div>
    <div>
      <a href="#" onclick="logout()">Logout</a>
    </div>
  </div>

  <div class="container">
    <h2>Your Profile</h2>
    <div class="profile-card" id="profileInfo">
      Loading profile...
    </div>
  </div>

  <script>
    const token = localStorage.getItem("access_token");

    if (!token) {
      window.location.href = "/login/";
    }

    fetch("/api/user/profile/", {
      method: "GET",
      headers: {
        "Authorization": `Bearer ${token}`,
        "Content-Type": "application/json"
      }
    })
    .then(res => {
      if (!res.ok) {
        throw new Error("Unauthorized or token expired");
      }
      return res.json();
    })
    .then(data => {
      document.getElementById("profileInfo").innerHTML = `
        <p><strong>Name:</strong> ${data.name || 'N/A'}</p>
        <p><strong>Email:</strong> ${data.email}</p>
        <p><strong>Phone:</strong> ${data.phone}</p>
        <p><strong>DOB:</strong> ${data.dob}</p>
        <p><strong>Gender:</strong> ${data.gender}</p>
        <p><strong>Address:</strong> ${data.address}</p>
      `;
    })
    .catch(err => {
      console.error(err);
      localStorage.removeItem("access_token");
      window.location.href = "/login/";
    });

    function logout() {
      localStorage.removeItem("access_token");
      window.location.href = "/login/";
    }
  </script>
</body>
</html>

<!-- 
