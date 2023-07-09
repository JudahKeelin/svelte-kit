<script lang="ts">
    import { page } from '$app/stores';
    import { onMount } from 'svelte';
	import moment from 'moment';
    import logo from '$lib/logo.png';


    const userId = $page.params.userId;

    console.log(userId);

    let name = '';

    const getName = async () => {
        const res = await fetch('https://simplecomplex-production.up.railway.app/api/users/name/' + userId);
        const json = await res.text();
        name = json;
        console.log(name);
        return json;
        
    };

    let occupancy:any;
    let prospects:any;
    let sales:any;
    let singleBed:any;
    let doubleBed:any;
    let tripleBed:any;
    let date = moment().startOf('day').format('YYYY-MM-DD');
    let result = '';

    async function handleUpdate () {

        console.log(userId);
        console.log(name);
        console.log(date);
        console.log(occupancy);
        console.log(prospects);
        console.log(sales);
        console.log(singleBed);
        console.log(doubleBed);
        console.log(tripleBed);

		const res = await fetch('https://simplecomplex-production.up.railway.app/api/updates/save', {
			method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
			body: JSON.stringify({
                "userId": userId,
                "complexName": name,
                "date": date,
                "occupancy":occupancy,
                "prospects": prospects,
                "sales": sales,
                "oneBedPrice": singleBed,
                "twoBedPrice": doubleBed,
                "threeBedPrice": tripleBed
			})
		})
		
        const json = await res;
    
        occupancy = null;
        prospects = null;
        sales = null;
        singleBed = null;
        doubleBed = null;
        tripleBed = null;

        setTimeout(() => {
            window.location.href = "/dashboard/" + userId;
        }, 1000);
	}

</script>


<style>
    .col-4 {
        padding-top: 2vh;
    }
    .mainBody {
        background-image: url("http://iheartbackgrounds.weebly.com/uploads/1/7/4/2/17420453/3108862_orig.jpg");
        height: 100vh;
        -webkit-text-fill-color: aliceblue;
    }

    .form-control {
        -webkit-text-fill-color: black;
    }

    .main-logo {
        width: 5rem;
        height: auto;
    }

    .h3 {
        margin-top: 14vh;
        margin-bottom: 5vh;
    }

    .spacer {
        font-size: 1px;
    }
</style>


<nav class="navbar bg-body-tertiary">
    <div class="container-fluid">
       
            <div class="col-1">
                <img src={logo} class="image-fluid main-logo" alt="">
            </div>
            <div class="col-5 RobotoMedium">
                <h1>THE MULTI-FAMILY MARKET</h1>
            </div>
            <div class="col-4"></div>
            <div class="col-1">
                <button class="btn btn-secondary" on:click={() => window.location.href = "/dashboard/" + userId}>Dashboard</button>
            </div>
            <div class="col-1">
                <button class="btn btn-secondary" on:click={() => window.location.href = "/"}>Logout</button>
            </div>
    </div>
</nav>

<div class="container-fluid text-center mainBody">
    {#await getName()}
        <p>...loading</p>
    {:catch error}
        <p style="color: red">{error.message}</p>
    {/await}

    <div class="row spacer">
        <p>l</p>
    </div>

    <div class="div h3">
        <h3>Weekly Update Form</h3>
    </div>

    

    <form on:submit|preventDefault={handleUpdate}>



        <div class="row">

            <div class="col"></div>

            <div class="col-3">

                <div class="mb-3">
                    <label for="inputOccupancy" class="form-label">Occupancy %</label>
                    <input type="Occupancy" class="form-control" id="Occupancy" bind:value={occupancy} aria-describedby="Occupancy">
                    <div id="OccupancyHelp" class="form-text">Current percentage of units occupied.</div>
                </div>

            </div>

            <div class="col-3">

                <div class="mb-3">
                    <label for="inputProspects" class="form-label">Prospects</label>
                    <input type="Prospects" class="form-control" id="Prospects" bind:value={prospects}>
                    <div id="ProspectsHelp" class="form-text">Number of prospective tenants in last 7 days.</div>
                </div>

            </div>

            <div class="col-3">

                <div class="mb-3">
                    <label for="inputSales" class="form-label">Sales</label>
                    <input type="Sales" class="form-control" id="Sales" bind:value={sales}>
                    <div id="SalesHelp" class="form-text">Number of sales in last 7 days.</div>
                </div>

            </div>

            <div class="col"></div>

        </div>



        <div class="row">

            <div class="col"></div>

            <div class="col-3">

                <div class="mb-3">
                    <label for="inputSingleBed" class="form-label">Single Bedroom Price</label>
                    <input type="SingleBed" class="form-control" id="SingleBed" bind:value={singleBed}>
                    <div id="SingleBedHelp" class="form-text"></div>
                </div>

            </div>

            <div class="col-3">

                <div class="mb-3">
                    <label for="inputDoubleBed" class="form-label">Two Bedroom Price</label>
                    <input type="DoubleBed" class="form-control" id="DoubleBed" bind:value={doubleBed}>
                    <div id="DoubleBedHelp" class="form-text"></div>
                </div>

            </div>

            <div class="col-3">

                <div class="mb-3">
                    <label for="inputTripleBed" class="form-label">Three Bedroom Price</label>
                    <input type="TripleBed" class="form-control" id="TripleBed" bind:value={tripleBed}>
                    <div id="TripleBedHelp" class="form-text"></div>
                </div>

            </div>

            <div class="col"></div>

        </div>

        <div class="div tips">
            <p>If you don't have any data for a given field, enter 0.</p>
            <p>Please use whole numbers.</p>
        </div>

        <div class="row">

            <div class="col"></div>
            <div class="col">

                <button type="submit" class="btn btn-primary">Submit</button>

            </div>
            <div class="col"></div>

        </div>

    </form>
   
</div>