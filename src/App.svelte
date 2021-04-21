<svelte:head>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
  <link rel="stylesheet" href="https://unpkg.com/chota@latest">
</svelte:head>
<script>
  import { onMount } from "svelte";
  export let date;
  import RangeSlider from "svelte-range-slider-pips";
  let api_output ={};

  const moods = ["Strong Sell","😫","😫","😫","😥","Sell","😥","😐","😐","😐","Hold","😐","😀","😀","😀","Buy","😀","😁","😁","😁","Strong Buy"];

  onMount(async () => {
    const res = await fetch("/api/date");
    const newDate = await res.text();
    date = newDate;
    calculateKelly();
  });
  var my_kelly = [0];
  var show_kelly = [0];
  var fat_kelly = [0];
  var friend_kelly = [0];
  let gain_chance=0;
  let ticker = 'TSLA';
  let varx = 0;

import {
  Auth0Context,
  Auth0LoginButton,
  Auth0LogoutButton,
  authError,
  authToken,
  idToken,
  isAuthenticated,
  isLoading,
  login,
  logout,
  userInfo,
} from '@dopry/svelte-auth0';

function currencyFormat(num,decimals) {
  return num.toFixed(decimals).replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,')
}
function calculateKelly() {
    my_kelly = "no";
    ticker = ticker.toUpperCase();
    fetch("https://www.insuremystock.com/options/kelly/"+ticker)
        .then(d => d.text())
        .then(d =>
        {
            api_output = JSON.parse(d);
            console.log(api_output);
            if ('error' in api_output)
                my_kelly="error";
            else
            {
                show_kelly = [(api_output.kelly_k*100)];
                my_kelly = (api_output.kelly_k);
                friend_kelly = [(api_output.kelly_k*100-2)];
                fat_kelly = [(api_output.kelly_k*100)];
                gain_chance = Math.round(api_output.prob_up*100);
                varx = api_output.prob_down_n/api_output.prob_up_n;

            }
        });

}
function updateClipboard(newClip) {
  navigator.clipboard.writeText(newClip).then(function() {
    window.open("https://www.robinhood.com/stocks/"+ticker);
  }, function() {
    /* clipboard write failed */
  });
}

let post_url = encodeURIComponent("https://social.oracled.com/?symbol=");
let post_title =  encodeURIComponent("Check it out: I just FOMO optimized ");
</script>
<style>
body {
max-width:90rem;
margin:0 auto;
}
.card{
margin:1rem auto;
}
</style>
<!--<h1> Test avataar </h1>
<img src='https://avataaars.io/?avatarStyle=Circle&topType=WinterHat4&accessoriesType=Sunglasses&hatColor=PastelRed&facialHairType=BeardMedium&facialHairColor=Auburn&clotheType=CollarSweater&clotheColor=Pink&eyeType=Dizzy&eyebrowType=UpDown&mouthType=Smile&skinColor=Pale'
/>

<Auth0Context domain="dev-gh9on756.us.auth0.com" client_id="lDh9u5tdu1Kk5CkXtZjmjjmUKuGARk0v">
  <Auth0LoginButton class="btn">Login</Auth0LoginButton>
  <Auth0LogoutButton class="btn">Logout</Auth0LogoutButton>
  <pre>isLoading: {$isLoading}</pre>
  <pre>isAuthenticated: {$isAuthenticated}</pre>
  <pre>authToken: {$authToken}</pre>
  <pre>idToken: {$idToken}</pre>
  <pre>userInfo: {JSON.stringify($userInfo, null, 2)}</pre>
  <pre>authError: {$authError}</pre>
</Auth0Context>
-->


{$isAuthenticated}
<body>
  <h1> Login and get Oracled </h1>
  <Auth0Context domain="dev-gh9on756.us.auth0.com" client_id="lDh9u5tdu1Kk5CkXtZjmjjmUKuGARk0v">
  <Auth0LoginButton class="btn">Login</Auth0LoginButton>
  <Auth0LogoutButton class="btn">Logout</Auth0LogoutButton>
</Auth0Context>
<div class="row card">
  <div class="col-2"></div>
  <div class="col-8">
    <iframe width="100%" height="420"  src="https://public.com/stocks/{ticker}/embed" frameborder="0" allow="encrypted-media" allowfullscreen allowtransparency></iframe>
  </div>
    <div class="col-2"></div>
</div>
  
{#if {$isAuthenticated}
<div class="row card">
  <div class="col-3"><img src='https://avataaars.io/?avatarStyle=Circle&topType=ShortHairTheCaesarSidePart&accessoriesType=Kurt&hairColor=Brown&facialHairType=BeardMajestic&facialHairColor=BrownDark&clotheType=BlazerShirt&eyeType=Default&eyebrowType=Angry&mouthType=Serious&skinColor=Pale'
                          width="50" /> <br> Fat Tony </div>
  <div class="col-6"><RangeSlider  disabled={true} float pips all='label' bind:values={fat_kelly}  pipstep={5} min={-10} max={10} formatter={ v => moods[v+10] }/></div>
  <div class="col-3 text-center "><span style="font-size:4rem;color:purple;">{gain_chance}%</span></div>
</div>
<div class="row card">
  <div class="col-3"><img src='https://avataaars.io/?avatarStyle=Circle&topType=LongHairStraightStrand&accessoriesType=Round&hairColor=Platinum&facialHairType=Blank&clotheType=ShirtVNeck&clotheColor=Pink&eyeType=EyeRoll&eyebrowType=UnibrowNatural&mouthType=Twinkle&skinColor=Tanned'
                          width="50" /><br> Friends </div>
  <div class="col-6"><RangeSlider  disabled={true} float pips all='label' bind:values={friend_kelly}  pipstep={5} min={-10} max={10} formatter={ v => moods[v+10] }/></div>
  <div class="col-3 text-center "><span style="font-size:4rem;color:purple;">{gain_chance-3}%</span></div>
</div>
<div class="row card">
  <div class="col-3"><img src={$userInfo["picture"]} width="50" /> <br> {$userInfo["email"]} </div>
  <div class="col-6"><RangeSlider float pips all='label'  bind:values={show_kelly}  pipstep={5} min={-10} max={10} formatter={ v => moods[v+10] }/></div>
  <div class="col-3 text-center "><span style="font-size:4rem;color:purple;">{Math.round(((3*show_kelly/100)+varx)*100/(1+varx))}%</span> <br> Gain Odds </div>
</div>
<a style="color:#168ed7;font-size:2rem;" href="https://twitter.com/share?url={post_url}{ticker}&text={post_title}{ticker}&hashtags=fomo,oracled.com" class="button fa fa-twitter pull-left"></a>
<button class="text-white bg-dark pull-right" on:click={updateClipboard(show_kelly)}>Trade</button>
{/if}
</body>
