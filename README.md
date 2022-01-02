
<!DOCTYPE html>
<html >
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Token sale page">

    <title>Token sale</title>
   
    <script>
        var test = false; // False if contracts are on Mainnet
        var contractAddressSale = '0x39128d65E5bb5FFC5A03b0d2044B2802BFdCF4d1';
        var contractAddressToken = '0x1Cd55F661E809bEeBe51A2c48eA0764dA6D5e073';
    </script>
    
    <style>
        
        body {font-family: Arial, "Helvetica Neue", Helvetica, sans-serif; color: #FFFFFF; background-color: #000000; font-size: 16px; font-weight: 400;}

        h1 { font-size: 24px; font-weight: 700;}
        h2 { font-size: 22px; font-weight: 500;}

        .small {
            font-size: 12px;
        }

        .err {
             color: red;
        }
        
        .green {
             color: green;
        }
        
        .blue {
             color: blue;
        }

        * {
          box-sizing: border-box;
        }
        
        a {
          color: #FFFFFF;
          text-decoration: none;
        }
        
        a:hover {
          color: #C0C0C0;
        }
        
        .clickable {
            cursor: pointer;
        }
        
        .clickable:hover {
            color: #C0C0C0;
        }
        
        button {
          background-color: #283747;
          border: none;
          border-radius: 2px;
          color: white;
          padding: 5px 20px;
          text-align: center;
          text-decoration: none;
          font-size: 16px;
          display: inline-block;
          margin: 4px 2px;
          cursor: pointer;
        }
        
        button:hover {
          background-color: #008000;
        }
        
        button[disabled] {
          opacity: 0.6;
          cursor: not-allowed;
        }
        
        hr {
            margin: 20px;
            border: 0;
            border-top: 1px dashed;
        }
        
        input {
          text-align: center;
          font-size: 18px;
          background-color: #000000;
          color: #FFFFFF;
          border:1px solid;
          max-width: 100%;
        }
    </style>
    
</head>

<body>
    
    <div style="text-align: center">
        <button id="connect" style="font-size: 12px">Connect</button> <button class="switch" id="" style="font-size: 12px;"></button>
        <span id="nometamask" class="err" style="display: none">Please install Metamask first...</span>
        <div class="network small"><span id="curnet"><span class="err">Please use DApp browser/extension (e.g. <a target="_blank" href="https://metamask.io">Metamask</a>)</span></span> <span id="myAddr"></span>
        <span id="referred" style="display:none"><br>Referrer: <span id="referrer"></span></span></div>
    </div>
    
    <div style="text-align: center">
        <h1>Token info</h1>
        <h2><span id="tokenName">Mint</span> (<span class="tokenSymbol">MNT</span>)</h2>
        <p><a target="_blank" href="https://testnet.bscscan.com/token/0x1Cd55F661E809bEeBe51A2c48eA0764dA6D5e073" id="tokenAddress">0x1Cd55F661E809bEeBe51A2c48eA0764dA6D5e073</a></p>
        <!-- Reserved in case you want to show decimals and total supply: Decimals <span id="#tokenDecimals"></span> Total supply <span id="#tokenSupply"></span>-->
        <p>Do not send BNB to the token contract!</p>
        <p><button id="addToken" style="text-align: center">Add to Metamask</button> <button id="copyToken" style="text-align: center">Copy address</button></p>
    </div>
    <hr>
    
    <div style="text-align: center">
        <h1>Token sale status</h1>
        <h1>
            <span id="finished" style="display:none" class="status green">Finished</span>
            <span id="addtokens" style="display:none" class="status err"><br>Ask token sale admin to approve token sale contract or check tokens balance on the wallet!</span>
            <span id="active" style="display:none" class="status green">Active</span>
            <span id="paused" style="display:none" class="status err">Paused</span>
            <span id="upcoming" style="display:none" class="status blue">Upcoming</span>
            <span id="upcomingtimer" style="display:none" class="status blue"><br>begins in <span id="starttimer"></span></span>
            <span id="activetimer" style="display:none" class="status green"><br>ends in <span id="endtimer"></span></span>
        </h1>
        <p><progress id="progress" value="0" max="100" style="width: 70%"></progress></p>
        <p>Raised: <span id="raised"></span> of <span class="hardcap"></span> BNB</p>
        <p>Tokens sold: <span id="sold"></span> of <span class="saleqty"></span> <span class="tokenSymbol">MNT</span></p>
        <p>Remaining: <span id="toraise"></span> BNB (~ <span id="unsold"></span> <span class="tokenSymbol">MNT</span>)</p>
    </div>
    <hr>
    
    <div style="text-align: center">
        <h1>Buy tokens</h1>
        <p>1 BNB = <span class="rate"></span> <span class="tokenSymbol">MNT</span></p>
        <p><input type="number" id="buyAmount" value="0" min="0"> BNB</p>
        <p>You get: <span id="get">0</span> <span class="tokenSymbol">MNT</span></p>
        <p><button id="buyBtn" style="text-align: center">Buy</button></p>
        <p>You have contributed <span id="mySpendings">?</span> of <span class="max">?</span> BNB maximum purchase limit, 
        can spend max. <span id="myRemaining">?</span> BNB more</p>
        <p>In my wallet: <span id="myTokens"></span> <span class="tokenSymbol">MNT</span></p>
    </div>
    <hr>
    
    <div style="text-align: center">
        <h1>Token sale information</h1>
        <p>Hardcap: <span class="hardcap"></span> BNB (~ <span class="saleqty"></span> <span class="tokenSymbol">MNT</span>)</p>
        <p>Rate: 1 BNB = <span class="rate"></span> <span class="tokenSymbol">MNT</span> (~ <span class="price"></span> BNB/<span class="tokenSymbol">MNT</span>)</p>
        <p>Purchase limit: <span class="max"></span></p>
        <p>Start: <span id="start"></span></p>
        <p>End: <span id="end"></span></p>
        <p>Duration: <span id="duration"></span></p>

    </div>
    <hr>
    
    <div style="text-align: center">
        <h1>Sale contract</h1>
        
    </div>
    <hr>
    
    <div id="refarea" style="text-align: center">
        <h1>Referral program</h1>
        <p>Share your referral link and get paid instantly to your wallet for every referred token purchase.</p>
        <p>Total paid to referrers: <span id="refTotal"></span> BNB</p>
        <p>Referral commission: <span id="refPercent">10</span>%</p>
        <p>Your referral earnings: <span id="refMy"></span> BNB</p>
        
        <p>Share your referral link or QR code and get commission for referred token purchases instantly to your wallet.</p>
        <p><input type="text" id="referLink" size="70" readonly="true"> <button id="copyreflink">Copy link</button></p>
        <div id="refqrcode">
            <div style="text-align: center" id="refqr"></div>
        </div>
        <p id="refErr" class="err" style="display: none">Please connect your wallet on Binance Smart Chain to generate your referral link!</p>
    </div>
    
<script src='https://dappbuilder.org/js/jquery-3.6.0.min.js' type="text/javascript" charset="utf-8"></script>
<script src='https://dappbuilder.org/js/ethers-5.0.umd.min.js' type="text/javascript" charset="utf-8"></script>
<script src='https://dappbuilder.org/bsc/tokensaletimerefadm/js/tokensale.ui.js' type="text/javascript" charset="utf-8"></script>

</body>
</html>

