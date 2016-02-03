
<style type="text/css">
    .index-doc{
        background: url(./images/index-sdk.png) no-repeat 10% center; height: 110px;
        margin-right: 10px;
        padding-left: 30%; padding-right: 10%; padding-top: 32px; margin-bottom: 10px;
    }
    .index-doc .title{
        font-size: 16px; font-weight: bold; padding-top: 10px; padding-bottom: 20px; 
    }
    .index-doc .intro{
        font-size: 12px;
    }
    .index-native{
        background: url(./images/index-mod.png) no-repeat 10% center; height: 110px;
        margin-right: 10px;
        padding-left: 30%; padding-right: 10%; padding-top: 32px; margin-bottom: 10px;
    }
    .index-native .title{
        font-size: 16px; font-weight: bold; padding-top: 10px; padding-bottom: 20px; 

    }
    .index-native .intro{
        font-size: 12px;
    }
    
    #overview{
        color:#fff; overflow: hidden;
        margin-left: 25px;
        margin-right: 25px; margin-bottom: 20px;
    }
    #overview a{display: block; color: #fff; text-decoration: none;}
    #overview .first{
        float: left; overflow: hidden; width: 33%; height: 120px; padding-bottom: 10px;
    }
    #overview .outer{
        float: right; overflow: hidden; width: 60%;
    }
    #overview .outer .title{
        height: 100%; margin-right: 10px; line-height: 110px;
        padding-left: 33%;
    }
    #overview .outer .last .title{
        margin-right: 0;
    }
    #overview .outer .wrap{
        float: left; width: 33.33%; height: 120px; padding-bottom: 10px;
    }
    .blue1{
        background-color: #6ebfda;
    }
    .blue1:hover{
        background-color: #8dd4eb;
    }
    .blue2{
        background-color: #709bce;
    }
    .blue2:hover{
        background-color: #87b2e4;
    }
    .purple{
        background-color: #9699e0;
    }
    .purple:hover{
        background-color: #acaff1;
    }
    .purple2{
        background-color: #8C91EF;
    }
    .purple2:hover{
        background-color: #acaff1;
    }
    .green{
        background-color: #48c79c;
    }
    .green:hover{
        background-color: #64e2b8;
    }
	.yellow{
        background-color: #bdb768;
    }
    .yellow:hover{
        background-color: #d2b48c;
    }
	.gold{
        background-color: #ffa500;
    }
    .gold:hover{
        background-color: #deb887;
    }    
        
</style>

<br/>

<p align=center><img alt="" src="./images/juma.png" /></p>

<!-- 添加文档中心搜索功能 -->

<form class="search-docs" target="_blank" action="http://zhannei.baidu.com/cse/search">
    <input type="text" placeholder="搜索 JUMA 文档中心" class="search_content" id="bdcsMain" name="q">
    <button type="submit" class="search-btn">搜索文档</button>
</form>

<br/>
<br/>
<br/>


<div id="overview">
<div class="first">
        <a class="wrap blue1 index-doc" href="../embedded_api/guide/">
            <div class="title">嵌入式SDK</div>
        </a>
</div>
<div class="first">
        <a class="wrap purple2 index-doc" href="../android_api/guide/">
            <div class="title">Android SDK</div>
        </a>
</div>
<div class="first">
        <a class="wrap green index-doc" href="../ios_api/guide/">
            <div class="title">iOS SDK</div>
        </a>
</div>
</div>
                        
<div id="overview">
<div class="first">
        <a class="wrap yellow index-native" href="../stm32_platform/guide/">
            <div class="title">STM32平台</div>
        </a>
</div>
<div class="first">
        <a class="wrap gold index-native" href="../nrf51_platform/guide/">
            <div class="title">nRF51平台</div>
        </a>
</div>

</div>