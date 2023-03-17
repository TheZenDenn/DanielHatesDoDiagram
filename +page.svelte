<svelte:head>
<script src="https://cdn.plot.ly/plotly-2.18.2.min.js" type="text/javascript"></script>
</svelte:head>
<script>   
import { onMount } from 'svelte' 
let dato="2023-03-16" 
let totdobesøk=0  
let feeds=[]   
let romoversikt=[]
    romoversikt[0]={romnummer:"152",antall:0, antalldato:0};
    romoversikt[1]={romnummer:"153",antall:0, antalldato:0};
    romoversikt[2]={romnummer:"154",antall:0, antalldato:0};
    romoversikt[3]={romnummer:"157",antall:0, antalldato:0};
    romoversikt[4]={romnummer:"158",antall:0, antalldato:0};
    romoversikt[5]={romnummer:"146",antall:0, antalldato:0};
    romoversikt[6]={romnummer:"147",antall:0, antalldato:0};
    romoversikt[7]={romnummer:"148",antall:0, antalldato:0};
    romoversikt[8]={romnummer:"240",antall:0, antalldato:0};
    romoversikt[9]={romnummer:"241",antall:0, antalldato:0};
    romoversikt[10]={romnummer:"242",antall:0, antalldato:0};
    romoversikt[11]={romnummer:"243",antall:0, antalldato:0};
    romoversikt[12]={romnummer:"234",antall:0, antalldato:0};
    romoversikt[13]={romnummer:"245",antall:0, antalldato:0};
    romoversikt[14]={romnummer:"246",antall:0, antalldato:0};
    romoversikt[15]={romnummer:"247",antall:0, antalldato:0};
    romoversikt[16]={romnummer:"231",antall:0, antalldato:0};
    romoversikt[17]={romnummer:"228",antall:0, antalldato:0};
    romoversikt[18]={romnummer:"226",antall:0, antalldato:0};
    romoversikt[19]={romnummer:"230",antall:0, antalldato:0};
    romoversikt[20]={romnummer:"350",antall:0, antalldato:0};
    romoversikt[21]={romnummer:"354",antall:0, antalldato:0};
    romoversikt[22]={romnummer:"336",antall:0, antalldato:0};
    romoversikt[23]={romnummer:"337",antall:0, antalldato:0};
    romoversikt[24]={romnummer:"338",antall:0, antalldato:0};
    romoversikt[25]={romnummer:"339",antall:0, antalldato:0};
    romoversikt[26]={romnummer:"340",antall:0, antalldato:0};
    romoversikt[27]={romnummer:"341",antall:0, antalldato:0};
    romoversikt[28]={romnummer:"321",antall:0, antalldato:0};
    romoversikt[29]={romnummer:"322",antall:0, antalldato:0};
    romoversikt[30]={romnummer:"323",antall:0, antalldato:0};
    romoversikt[31]={romnummer:"324",antall:0, antalldato:0};
    romoversikt[32]={romnummer:"325",antall:0, antalldato:0};



//test=romoversikt.map(x => x.romnummer);
    
        
const hentData=async () => {
    const data =await fetch("https://api.thingspeak.com/channels/2022245/feeds.json")
    const json= await data.json()
    feeds=json.feeds   
 }

 
onMount(async () => {
    await hentData()
    //Nå kan du gjøre hva du vil under her Dennis!           
    for (let j=0;j<=romoversikt.length-1;j++) {
        for (let i=0;i<=feeds.length-1;i++) {
            if (feeds[i].field1 == romoversikt[j].romnummer) {
                    romoversikt[j].antall++;
        }   if (feeds[i].field1 == romoversikt[j].romnummer && feeds[i].created_at.substr(0,10) == dato) {
                    romoversikt[j].antalldato++;
        }
        }
    }
    
    
    
    const data = [];
for (let i = 0; i < romoversikt.length; i++) {
  data.push({
    x: [romoversikt[i].romnummer],
    y: [romoversikt[i].antall],
    type: 'bar'
  });
}

const datadato = [];
for (let i = 0; i < romoversikt.length; i++) {
  data.push({
    x: [romoversikt[i].romnummer],
    y: [romoversikt[i].antalldato],
    type: 'bar'
  });
}
Plotly.newPlot('divtot', data);
Plotly.newPlot('divdato', datadato);
    })

    
 const sorterdato=()=> {
    totdobesøk=0
    for (let i=0;i<=feeds.length-1;i++) {
        if (feeds[i].created_at.substr(0,10) == dato) {
                totdobesøk++;
            }
    }
    for (let i=0;i<=romoversikt.length-1;i++) {
        romoversikt[i].antalldato=0
        romoversikt[i].antall=0
    }

    for (let j=0;j<=romoversikt.length-1;j++) {
        for (let i=0;i<=feeds.length-1;i++) {
            if (feeds[i].field1 == romoversikt[j].romnummer) {
                    romoversikt[j].antall++;
        }   if (feeds[i].field1 == romoversikt[j].romnummer && feeds[i].created_at.substr(0,10) == dato) {
                    romoversikt[j].antalldato++;
        }
        }
    } 
}
</script> 
<style>   

 td,th,table {
        text-align:center;
        border:5px solid #67236A;
        border-collapse: collapse;
        padding:10px;
        color: #276A23;
    }
    table {
        position: absolute;
        left:300px;
        top:300px;
        width:50%    }
    h1 {
        position: absolute;
        left:600px;
        top:200px;

    }
    #divtot {
        position: absolute;
        top:400px;
    }

</style>
<h1>Oversikt over måledata</h1>
<label for="Dato">Dato:</label>
  <input type="date" id="Dato" name="Dato" bind:value={dato} on:change={sorterdato}>
<p>{dato}
{totdobesøk}
</p>

<table> 
    <tr>    
        <th>Måledato:</th>    
        <th>klokkeslett:</th>           
        <th>rom nummer:</th>    
    </tr>     
{#each feeds as field}
    <tr>       
        <td>{field.created_at.substr(8,2)+"/"+field.created_at.substr(5,2)+"-"+field.created_at.substr(0,4)}</td>
        <td>{field.created_at.substr(11,8)}</td>        
        <td>{field.field1}</td>      
    </tr>        
{/each}   
</table>
<table style="margin-top:230%">
    <tr>
    <th>Rom Nummer</th>
    <th>Antall Besøk</th>
    </tr>
{#each romoversikt as rom}
    <tr>       
        <td>{rom.romnummer}</td>
        <td>{rom.antall}</td>         
    </tr>        
{/each}  
 </table>


<table style="margin-top:450%">
    <tr>
    <th>Rom Nummer {dato}</th>
    <th>Antall Besøk</th>
    </tr>
{#each romoversikt as rom}
    <tr>       
        <td>{rom.romnummer}</td>
        <td>{rom.antalldato}</td>         
    </tr>        
{/each}  
 </table>

<div id="divtot"></div>
<div id="divdato"></div>