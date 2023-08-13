<script>
    import logo from '$lib/logo.png'


    let email = '';
    let password = '';
    let invalid = false;
    let user_id = 0;

    const handleLogin = async () => {
        invalid = false;
        const res = await fetch('https://simplecomplex-production.up.railway.app/api/users/find?email=' + email + '&password=' + password);
        const json = await res.json();
        const response = res.status;
        user_id = json;        
        console.log(user_id);
        if (response == 200) {
            console.log("success");
            window.location.href = "/dashboard/" + user_id;
        } else {
            invalid = true;
        }
    }

</script>

<style>
    .col-4 {
        margin-top: 16vh;
    }

    .mainBody {
        background-image: url("http://iheartbackgrounds.weebly.com/uploads/1/7/4/2/17420453/3108862_orig.jpg");
        height: 100vh;
        -webkit-text-fill-color: rgb(255, 255, 255);
    }

    .form-control {
        -webkit-text-fill-color: black;
    }

    .alert {
        margin-top: 2vh;
        opacity: 1;
    }

    .main-logo {
        width: 8rem;
        height: auto;
    }

    button {
        padding-left: 10px;
        padding-right: 10px;
    }
</style>

<nav class="navbar bg-body-tertiary">
    <div class="container-fluid">
            <div class="col-1">
                <img src={logo} class="image-fluid main-logo" alt="">
            </div>
            <div class="col RobotoMedium">
                <h1>THE MULTI-FAMILY MARKET</h1>
            </div>
            <div class="col"></div>
    </div>
</nav>

<div class="container-fluid text-center mainBody">
    <form on:submit|preventDefault={handleLogin}>
        <div class="row text-center">
            <div class="col-4">
            </div>
            <div class="col-4">
                <h1>Login</h1>
                <div class="mb-3">
                <label for="inputEmail1" class="form-label">Email address</label>
                    <input type="email" class="form-control" id="inputEmail1" bind:value={email} aria-describedby="emailHelp">
                    <div id="emailHelp" class="form-text">We'll never share your email with anyone else.</div>
                </div>
                <div class="mb-3">
                    <label for="inputPassword1" class="form-label">Password</label>
                    <input type="password" class="form-control" id="inputPassword1" bind:value={password}>
                </div>
                {#if invalid}
                    <div class="alert alert-danger" role="alert">
                        Invalid email or password.
                    </div>
                {/if}
            </div>
            <div class="col-4"></div>
        </div>
        <div class="row">
            <div class="col"></div>
            <div class="col-1">
                <button type="submit" class="btn btn-primary">Login</button>
            </div>
            <div class="col-1">
                <button class="btn btn-secondary" on:click={() => window.location.href = "/newUser"}>New</button>
            </div>
            <div class="col-1">
                <button class="btn btn-info" on:click={() => window.location.href = "/dashboard/9436991"}>Demo</button>
            </div>
            <div class="col"></div>
        </div>
    </form>
</div>