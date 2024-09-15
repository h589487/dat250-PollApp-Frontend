<script>
    let username = '';
    let email = '';

function createUser() {
    fetch("http://localhost:8080/users", {
        method:"POST",
        body: JSON.stringify({
            name: username,
            email: email
        }),
        headers: {
            "Content-Type": "application/json"
        }
    }).then((response) => {
        if(response.status === 201) {
            username = "";
            email = "";
            console.log("successfully added a user")
        }
    }).catch((error) => {
        alert(error.message);
    })
}
</script>

<form on:submit|preventDefault={createUser}>
    <input bind:value={username} placeholder="Enter your name" />
    <input bind:value={email} placeholder="Enter your email" />
    <button type="submit"> Create</button>
</form>