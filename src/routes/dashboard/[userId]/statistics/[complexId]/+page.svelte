<script lang="ts">
    import { page } from '$app/stores';
    import moment from 'moment';
    import logo from '$lib/logo.png';
    import { onMount } from 'svelte';
    import { Chart } from 'chart.js/auto';

    let complexId = $page.params.complexId;
    let userId = $page.params.userId;


    let updates:any = [];
    let dates:any = [];
    let occupancy:any = [];
    let prospects:any = [];
    let sales:any = [];
    let singleBed:any = [];
    let doubleBed:any = [];
    let tripleBed:any = [];
    let complexName = '';


    const getUpdates = async () => {
        const res = await fetch('https://simplecomplex-production.up.railway.app/api/updates/' + complexId);
        const json = await res.json();
        json.sort((a:any,b:any) => (a.date < b.date) ? 1 : -1);
        
        return json;
    };


    onMount(async () => {

        updates = await getUpdates();

        dates.push(0);
        occupancy.push(0);
        prospects.push(0);
        sales.push(0);
        singleBed.push(0);
        doubleBed.push(0);
        tripleBed.push(0);
        complexName = updates[0].complexName;

        for(let i = 0; i < updates.length; i++){
            console.log(updates[i]);
            tripleBed.push(updates[i].threeBedPrice);
            doubleBed.push(updates[i].twoBedPrice);
            singleBed.push(updates[i].oneBedPrice);
            sales.push(updates[i].sales);
            prospects.push(updates[i].prospects);
            occupancy.push(updates[i].occupancy);
            dates.push(updates[i].date.slice(5));
        }

        setTimeout(() => {
            const ctx0:any = document.getElementById('occupancyChart');

            console.log(ctx0);

            new Chart(ctx0, {
            type: 'line',
            data: {
                labels: dates,
                datasets: [{
                    label: 'Occupancy %',
                    data: occupancy,
                    borderWidth: 1
                }]
            },
            options: {
                plugins: {
                    title: {
                        display: true,
                        text: 'Occupancy Chart'
                    }
                }
            }
            });



            const ctx1:any = document.getElementById('salesChart');

            console.log(ctx1);

            new Chart(ctx1, {
            type: 'line',
            data: {
                labels: dates,
                datasets: [
                {
                    label: 'Prospects',
                    data: prospects,
                    borderWidth: 1
                },
                {
                    label: 'Sales',
                    data: sales,
                    borderWidth: 1
                }]
            },
            options: {
                plugins: {
                    title: {
                        display: true,
                        text: 'Prospects and Sales'
                    }
                }
            }
            });

            const ctx2:any = document.getElementById('pricingChart');

            console.log(ctx2);

            new Chart(ctx2, {
            type: 'line',
            data: {
                labels: dates,
                datasets: [
                {
                    label: 'One Bed Price',
                    data: singleBed,
                    borderWidth: 1
                },
                {
                    label: 'Two Bed Price',
                    data: doubleBed,
                    borderWidth: 1
                },
                {
                    label: 'Three Bed Price',
                    data: tripleBed,
                    borderWidth: 1
                }]
            },
            options: {
                plugins: {
                    title: {
                        display: true,
                        text: 'Pricing'
                    }
                }
            }
            });
        });

    });

</script>

<style>

    .mainBody {
            background-image: url("http://iheartbackgrounds.weebly.com/uploads/1/7/4/2/17420453/3108862_orig.jpg");
            height: 100%;
            -webkit-text-fill-color: rgb(0, 0, 0);
    }

    .main-logo {
            width: 5rem;
            height: auto;
    }

    .Chart {
        background-color: black;
        margin-top: 5vh;
    }

    .h2 {
        margin-top: 3vh;
        -webkit-text-fill-color: aliceblue;
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
    <div class="row spacer">
        <p>l</p>
    </div>

    <div class="row h2">
        <h2>{complexName}</h2>
    </div>

    <div class="row">
        <div class="col-2"></div>

        <div class="col-8">
            <canvas class="Chart" id="occupancyChart" width="400" height="200"></canvas>
        </div>

        <div class="col-2"></div>
    </div>

    <div class="row">
        <div class="col-2"></div>

        <div class="col-8">
            <canvas class="Chart" id="salesChart" width="400" height="200"></canvas>
        </div>
        
        <div class="col-2"></div>
    </div>
    <div class="row">
        <div class="col-2"></div>

        <div class="col-8">
            <canvas class="Chart" id="pricingChart" width="400" height="200"></canvas>
        </div>
        
        <div class="col-2"></div>
    </div>

        
        
        
        
        
        
</div>


