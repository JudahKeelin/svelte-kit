<script lang="ts">
    import { page } from '$app/stores';
    import moment from 'moment';
    import logo from '$lib/logo.png';
    import { onMount } from 'svelte';
    import { Chart } from 'chart.js/auto';
	import { toggle_class } from 'svelte/internal';

    let date = moment().format('YYYY-MM-D');

    console.log(date);

    let userId = $page.params.userId;
    let numId = parseInt(userId);

    console.log(userId);

    let dashboard = "/dashboard/" + userId;
    let statistics = "/dashboard/" + userId + "/statistics/" + userId;
    let addUpdate = "/addUpdate/" + userId;
    let signOut = "/";



    let updates:any = [];

    const getUpdates = async () => {
        const res = await fetch('https://simplecomplex-production.up.railway.app/api/users/home/' + userId);
        const json = await res.json();
        json.sort((a:any,b:any) => (a.date < b.date) ? 1 : -1);
        
        return json;
    };


    let chartData:{name: string, occupancy: number}[] = [];
    let names:any = [];
    let current:any = [];


    onMount(async () => {
        updates = await getUpdates();
        
        for (let i = 0; i < updates.length; i++) {
            if (names.find((name:any) => name === updates[i].complexName) === undefined) {
                names.push(updates[i].complexName);
                chartData.push({name: updates[i].complexName, occupancy: updates[i].occupancy});
            }
        }

        chartData.sort((a,b) => (a.name < b.name) ? -1 : 1);

        for (let i = 0; i < updates.length; i++) {
            if (updates[i].userId === numId) {
                console.log(updates[i]);
                current = updates[i];
                break;
            }
        }

        setTimeout(() => {
            const ctx:any = document.getElementById('myChart');

            console.log(ctx);

            Chart.defaults.color = '#fff';
            Chart.defaults.borderColor = '#56595D';

            new Chart(ctx, {
            type: 'bar',
            data: {
                labels: chartData.map((data) => data.name),
                datasets: [
                    {
                        label: 'Occupancy %',
                        data: chartData.map((data) => data.occupancy),
                        borderWidth: 1
                    }
                ]
            },
            });

            
        });

        
    });

    

</script>

<style>

    .main {
        background-image: url("http://iheartbackgrounds.weebly.com/uploads/1/7/4/2/17420453/3108862_orig.jpg");
        -webkit-text-fill-color: white;
    } 

    .bi {
      vertical-align: -.125em;
      fill: currentColor;
    }

    .main-logo {
        width: 100%;
        height: auto;
    }

    .table {
        opacity: 75%;
        overflow: auto;
        height: 50vh;
        margin-left: 5vh;
        margin-bottom: 10vh;
    }

    a {
        text-decoration: none;
        -webkit-text-fill-color: rgb(13, 110, 253);
    }

    li {
        list-style-type: none;
    }

    canvas {
        background: rgba(32, 32, 32, 0.5);
    }

</style>


<nav class="navbar sticky-top bg-body-tertiary">

    <div class="container-fluid">
       
            <div class="col-1">
                <img src={logo} class="image-fluid main-logo" alt="">
            </div>

            <div class="col-7 RobotoMedium">
                <h1>Simple Complex</h1>
            </div>

            <div class="col-4"></div>

    </div>

</nav>


<div class="container-fluid main" data-bs-theme="dark">
    <div class="row">
        <div class="sidebar border border-right col-md-3 col-lg-2 p-0 bg-body-tertiary" style="position: fixed; height: 100vh">
            <div class="sidebar-body offcanvas-md offcanvas-end bg-body-tertiary" tabindex="-1" id="sidebarMenu" aria-labelledby="sidebarMenuLabel">
                <div class="offcanvas-body d-md-flex flex-column p-0 pt-lg-3 overflow-y-auto">
                    <ul class="flex-column mb-auto">
                        <li class="">
                            <a class="link-primary icon-link d-flex align-items-center gap-2" aria-current="page" href={dashboard}>
                                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-house-door" viewBox="0 0 16 16">
                                    <path d="M8.354 1.146a.5.5 0 0 0-.708 0l-6 6A.5.5 0 0 0 1.5 7.5v7a.5.5 0 0 0 .5.5h4.5a.5.5 0 0 0 .5-.5v-4h2v4a.5.5 0 0 0 .5.5H14a.5.5 0 0 0 .5-.5v-7a.5.5 0 0 0-.146-.354L13 5.793V2.5a.5.5 0 0 0-.5-.5h-1a.5.5 0 0 0-.5.5v1.293L8.354 1.146ZM2.5 14V7.707l5.5-5.5 5.5 5.5V14H10v-4a.5.5 0 0 0-.5-.5h-3a.5.5 0 0 0-.5.5v4H2.5Z"/>
                                </svg>
                                Dashboard
                            </a>
                        </li>
                        <li class="">
                            <a class="link-primary icon-link d-flex align-items-center gap-2" href={addUpdate}>
                                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-input-cursor-text" viewBox="0 0 16 16">
                                    <path fill-rule="evenodd" d="M5 2a.5.5 0 0 1 .5-.5c.862 0 1.573.287 2.06.566.174.099.321.198.44.286.119-.088.266-.187.44-.286A4.165 4.165 0 0 1 10.5 1.5a.5.5 0 0 1 0 1c-.638 0-1.177.213-1.564.434a3.49 3.49 0 0 0-.436.294V7.5H9a.5.5 0 0 1 0 1h-.5v4.272c.1.08.248.187.436.294.387.221.926.434 1.564.434a.5.5 0 0 1 0 1 4.165 4.165 0 0 1-2.06-.566A4.561 4.561 0 0 1 8 13.65a4.561 4.561 0 0 1-.44.285 4.165 4.165 0 0 1-2.06.566.5.5 0 0 1 0-1c.638 0 1.177-.213 1.564-.434.188-.107.335-.214.436-.294V8.5H7a.5.5 0 0 1 0-1h.5V3.228a3.49 3.49 0 0 0-.436-.294A3.166 3.166 0 0 0 5.5 2.5.5.5 0 0 1 5 2z"/>
                                    <path d="M10 5h4a1 1 0 0 1 1 1v4a1 1 0 0 1-1 1h-4v1h4a2 2 0 0 0 2-2V6a2 2 0 0 0-2-2h-4v1zM6 5V4H2a2 2 0 0 0-2 2v4a2 2 0 0 0 2 2h4v-1H2a1 1 0 0 1-1-1V6a1 1 0 0 1 1-1h4z"/>
                                </svg>
                                Add Updates
                            </a>
                        </li>
                        <li class="">
                            <a class="link-primary icon-link d-flex align-items-center gap-2" href={statistics}>
                                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-graph-up" viewBox="0 0 16 16">
                                    <path fill-rule="evenodd" d="M0 0h1v15h15v1H0V0Zm14.817 3.113a.5.5 0 0 1 .07.704l-4.5 5.5a.5.5 0 0 1-.74.037L7.06 6.767l-3.656 5.027a.5.5 0 0 1-.808-.588l4-5.5a.5.5 0 0 1 .758-.06l2.609 2.61 4.15-5.073a.5.5 0 0 1 .704-.07Z"/>
                                </svg>
                                My Statistics
                            </a>
                        </li>
                    </ul>
        
                    <hr class="my-3">
        
                    <ul class="flex-column mb-auto">
                        <li class="">
                            <a class="link-primary icon-link d-flex align-items-center gap-2" href={signOut}>
                                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-box-arrow-left" viewBox="0 0 16 16">
                                    <path fill-rule="evenodd" d="M6 12.5a.5.5 0 0 0 .5.5h8a.5.5 0 0 0 .5-.5v-9a.5.5 0 0 0-.5-.5h-8a.5.5 0 0 0-.5.5v2a.5.5 0 0 1-1 0v-2A1.5 1.5 0 0 1 6.5 2h8A1.5 1.5 0 0 1 16 3.5v9a1.5 1.5 0 0 1-1.5 1.5h-8A1.5 1.5 0 0 1 5 12.5v-2a.5.5 0 0 1 1 0v2z"/>
                                    <path fill-rule="evenodd" d="M.146 8.354a.5.5 0 0 1 0-.708l3-3a.5.5 0 1 1 .708.708L1.707 7.5H10.5a.5.5 0 0 1 0 1H1.707l2.147 2.146a.5.5 0 0 1-.708.708l-3-3z"/>
                                </svg>
                                Sign out
                            </a>
                        </li>
                    </ul>

                    <hr class="my-3">
                    
                </div>

            </div>

            <div class="col" style="margin-left: 10px;">
                <div class="row">
                    <div class="col-7"><p>Occupancy:</p></div>
                    <div class="col">{current.occupancy}%</div>
                </div>
                <div class="row">
                    <div class="col-7"><p>Prospects:</p></div>
                    <div class="col">{current.prospects}</div>
                </div>
                <div class="row">
                    <div class="col-7"><p>Sales:</p></div>
                    <div class="col">{current.sales}</div>
                </div>
                <div class="row">
                    <div class="col-7"><p>One Bedroom:</p></div>
                    <div class="col">${current.oneBedPrice}</div>
                </div>
                <div class="row">
                    <div class="col-7"><p>Two Bedroom:</p></div>
                    <div class="col">${current.twoBedPrice}</div>
                </div>
                <div class="row">
                    <div class="col-7"><p>Three Bedroom:</p></div>
                    <div class="col">${current.threeBedPrice}</div>
                </div>

            </div>

        </div>
  
      <main class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
  
        <canvas class="my-4 w-100" id="myChart" width="877" height="370" style="display: block; box-sizing: border-box; height: 370px; width: 877px;"></canvas>
  
        <h2>All Updates</h2>
        <div class="table-responsive small" style="height: 50vh;">
          <table class="table table-striped table-sm">
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
                        <td><button type="button" class="btn btn-link" on:click={() => window.location.href = "/dashboard/" + userId + "/statistics/" + update.userId}>{update.complexName}</button></td>
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
      </main>
    </div>
  </div>