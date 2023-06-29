<script lang="ts">
    import { page } from '$app/stores';
    import moment from 'moment';
    import logo from '$lib/logo.png';
    import { onMount } from 'svelte';
    import { Chart } from 'chart.js/auto';

    let date = moment().format('YYYY-MM-D');

    console.log(date);

    const userId = $page.params.userId;

    console.log(userId);


    let updates:any = [];

    const getUpdates = async () => {
        const res = await fetch('https://simplecomplex-production.up.railway.app/api/users/home/' + userId);
        const json = await res.json();
        json.sort((a:any,b:any) => (a.date < b.date) ? 1 : -1);
        
        return json;
    };


    let names:any = [];
    let occupancy:any = [];
    let current:any = [];
    


    onMount(async () => {
        updates = await getUpdates();

        current = updates[0];

        for (let i = 0; i < updates.length; i++) {
            if (names.find((name:any) => name === updates[i].complexName) === undefined) {
                names.push(updates[i].complexName);
                occupancy.push(updates[i].occupancy);
            }
        }

        setTimeout(() => {
            const ctx:any = document.getElementById('myChart');

            console.log(ctx);

            new Chart(ctx, {
            type: 'bar',
            data: {
                labels: names,
                datasets: [{
                    label: 'Occupancy %',
                    data: occupancy,
                    borderWidth: 1
                }]
            },
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

    .table {
        opacity: 0.75;
    }

    .main-logo {
        width: 5rem;
        height: auto;
    }

    .mainChart {
        background-color: black;
        margin-top: 5vh;
        margin-bottom: 10vh;
        margin-left: 5vh;
    }

    .myTable {
        overflow: auto;
        height: 50vh;
        margin-left: 5vh;
        margin-bottom: 10vh;
    }

    .current {
        -webkit-text-fill-color: aliceblue;
        
    }

    .sidebar {
        margin-bottom: 1vh;
        border-color: aliceblue;
        border-style: solid;
        border-width: 1px;
        background-color: rgba(100,100,100, 0.5);
        border-radius: 10px;
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
            <div class="col-5"></div>
            <div class="col-1">
                <button class="btn btn-secondary" on:click={() => window.location.href = "/"}>Logout</button>
            </div>
    </div>
</nav>





<div class="container-fluid text-center mainBody">

    {#if updates.length > 0}
        <div class="row">
            <div class="col-3 current pt-2">
                <div class="row sidebar">
                    <h3>Current Statistics</h3>
                </div>
                <div class="row sidebar">
                    <h4>Occupancy</h4>
                    <h5>{current.occupancy}%</h5>
                </div>
                <div class="row sidebar">
                    <h4>Prospects</h4>
                    <h5>{current.prospects}</h5>
                </div>
                <div class="row sidebar">
                    <h4>Sales</h4>
                    <h5>{current.sales}</h5>
                </div>
                <div class="row sidebar">
                    <h4>One Bedroom Price</h4>
                    <h5>${current.oneBedPrice}.00</h5>
                </div>
                <div class="row sidebar">
                    <h4>Two Bedroom Price</h4>
                    <h5>${current.twoBedPrice}.00</h5>
                </div>
                <div class="row sidebar">
                    <h4>Three Bedroom Price</h4>
                    <h5>${current.threeBedPrice}.00</h5>
                </div>
                <div class="row sidebar"></div>
                <div class="row sidebar">
                    <button class="btn btn-secondary" on:click={() => window.location.href = "/addUpdate/" + userId}>Add Update</button>
                </div>

            </div>
            <div class="col-8">

                <canvas class="mainChart" id="myChart" width="400" height="200"></canvas>

                <div class="div myTable">
                    <table class="table table-striped over-flow-auto">
                            <thead>
                                <tr>
                                    <th scope="col">Date</th>
                                    <th scope="col">Complex</th>
                                    <th scope="col">Occupancy</th>
                                    <th scope="col">Prospects</th>
                                    <th scope="col">Sales</th>
                                    <th scope="col">One Bedroom</th>
                                    <th scope="col">Two Bedroom</th>
                                    <th scope="col">Three Bedroom</th>
                                </tr>
                            </thead>
                        <tbody>
                            {#each updates as update (update.id)}
                                <tr>
                                    <td>{update.date}</td>
                                    <td>{update.complexName}</td>
                                    <td>{update.occupancy}%</td>
                                    <td>{update.prospects}</td>
                                    <td>{update.sales}</td>
                                    <td>${update.oneBedPrice}.00</td>
                                    <td>${update.twoBedPrice}.00</td>
                                    <td>${update.threeBedPrice}.00</td>
                                </tr>
                            {/each}
                        </tbody>
                    </table>
                </div>
            </div>
            <div class="col-1"></div>
        </div>
    {/if}
</div>