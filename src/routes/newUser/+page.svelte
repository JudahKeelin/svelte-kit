<script lang="ts">
    import logo from '$lib/logo.png';



    let password:string;
    let confirmPassword:string;
    let invalid:boolean = false;
    let address:string;
    let city:string;
    let state:string;
    let zip:string;
    let country:string;
    let complexName:string;
    let email:string;
    let phone:string;
    let firstName:string;
    let lastName:string;
    let geography:any;
    let lat:number;
    let lng:number;
    

    
    async function getCoordinates () {
        let fullAddress = address + ' ' + city + ' ' + state + ' ' + zip + ' ' + country;
        fullAddress.replace(/ /g, '+');
        console.log(fullAddress);
        const res = await fetch('https://maps.googleapis.com/maps/api/geocode/json?address=' + fullAddress + '&key=AIzaSyAziaSeWiegbW8fAyYxJgLwoYj8WA8A5Sg');
        geography = await res.json();
        console.log(geography);
        setTimeout(() => {
            lat = geography.results[0].geometry.location.lat;
            lng = geography.results[0].geometry.location.lng;
            console.log(lat, lng);
        })
        return geography;
    }


    

    async function handleSubmit(){
        if(password === confirmPassword){
            invalid = false;
            getCoordinates();
            
            setTimeout(() => {
                console.log(
                    firstName,
                    lastName,
                    email,
                    password,
                    phone,
                    complexName,
                    address,
                    city,
                    state,
                    zip,
                    country,
                    lat,
                    lng
                )
            }, 1000)
            setTimeout(async () => {
                address = address.replace('+', ' ');
                const res = await fetch('https://simplecomplex-production.up.railway.app/api/users/add', {
					method: 'POST',
					headers: {
						'Content-Type': 'application/json'
					},
					body: JSON.stringify({
						"firstName": firstName,
						"lastName": lastName,
						"email": email,
						"password": password,
						"phoneNumber": phone,
						"complexName": complexName,
						"address": address,
						"city": city,
						"state": state,
						"zipCode": zip,
						"country": country,
						"latitude": lat,
						"longitude": lng
					})
				})
                window.location.href = "/";
            }, 3000)
        } else {
            invalid = true;
        }
        
    }
    
</script>



<style>
    .mainBody {
        background-image: url("http://iheartbackgrounds.weebly.com/uploads/1/7/4/2/17420453/3108862_orig.jpg");
        height: 100vh;
        -webkit-text-fill-color: aliceblue;
    }

    .form-control {
        -webkit-text-fill-color: black;
    }

    .main-logo {
        width: 8rem;
        height: auto;
    }

    
</style>



<nav class="navbar bg-body-tertiary">
    <div class="container-fluid">
            <div class="col-1">
                <img src={logo} class="image-fluid main-logo" alt="">
            </div>
            <div class="col-6 RobotoMedium">
                <h1>THE MULTI-FAMILY MARKET</h1>
            </div>
            <div class="col-5"></div>
    </div>
</nav>


<div class="container-fluid text-center mainBody">
    <h2>Create New User</h2>
    <form on:submit|preventDefault={handleSubmit}>
        <div class="row">

            <div class="col-3"></div>

            <div class="col-3">
                <div class="mb-3">
                    <label for="inputFirstName" class="form-label">First Name</label>
                    <input type="FirstName" class="form-control" id="FirstName" bind:value={firstName}>            
                </div>
                <div class="mb-3">
                    <label for="inputComplexName" class="form-label">Apartment Complex Name</label>
                    <input type="ComplexName" class="form-control" id="ComplexName" bind:value={complexName}>
                </div>
                
                <div class="mb-3">
                    <label for="inputPhone" class="form-label">Phone Number</label>
                    <input type="Phone" class="form-control" id="Phone" bind:value={phone}>
                </div>
            </div>

            <div class="col-3">
                <div class="mb-3">
                    <label for="inputLastName" class="form-label">Last Name</label>
                    <input type="LastName" class="form-control" id="LastName" bind:value={lastName}>            
                </div>
                <div class="mb-3">
                    <label for="inputEmail" class="form-label">Email</label>
                    <input type="Email" class="form-control" id="Email" bind:value={email}>
                </div>
                <div class="mb-3">
                    <label for="inputAddress" class="form-label">Address</label>
                    <input type="Address" class="form-control" id="Address" bind:value={address}>
                </div>
            </div>
            <div class="col-3">

            </div>
        </div>



        <div class="row">

            <div class="col"></div>

            <div class="col-2">
                <div class="mb-3">
                    <label for="inputCity" class="form-label">City</label>
                    <input type="City" class="form-control" id="City" bind:value={city}>
                </div>
            </div>

            <div class="col-1">
                <div class="mb-3">
                    <label for="inputState" class="form-label">State</label>
                    <input type="State" class="form-control" id="State" bind:value={state}>
                </div>
            </div>

            <div class="col-1">
                <div class="mb-3">
                    <label for="inputZip" class="form-label">Zip</label>
                    <input type="Zip" class="form-control" id="Zip" bind:value={zip}>
                </div>
            </div>

            <div class="col-1">
                <div class="mb-3">
                    <label for="inputCountry" class="form-label">Country</label>
                    <input type="Country" class="form-control" id="Country" bind:value={country}>
                </div>
            </div>

            <div class="col"></div>

        </div>



        <div class="row">

            <div class="col"></div>

            <div class="col">
                <div class="mb-3">
                    <label for="inputPassword" class="form-label">Password</label>
                    <input type="Password" class="form-control" id="Password" bind:value={password}>
                </div>
            </div>

            <div class="col"></div>

        </div>

        <div class="row">

            <div class="col"></div>

            <div class="col">
                <div class="mb-3">
                    <label for="inputConfirmPassword" class="form-label">Confirm Password</label>
                    <input type="ConfirmPassword" class="form-control" id="ConfirmPassword" bind:value={confirmPassword}>
                </div>
            </div>

            <div class="col"></div>

        </div>


        
        <button type="submit" class="btn btn-primary">Submit</button>
        {#if invalid}
            <div class="alert alert-danger" role="alert">
                Passwords do not match
            </div>
        {/if}
    </form>
</div>
