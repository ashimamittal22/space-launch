<template>
    <div id="root">
        <div  id="img_div">
            <img id="space_image" :src=item.links.mission_patch_small >
        </div>
        
        <h4 id="name">{{item.mission_name}} #{{item.flight_number}}</h4>
        <span style="width:100%" class="title">Mission Ids:</span>
            <ul style="height: 40px; color:#6772A4">
                <li v-for="(lItem, index) in item.mission_id" :key="index">{{lItem}}</li>
            </ul>
        <div class="data"><span class="title">Launch Year:</span><span class="value">{{item.launch_year}}</span></div>
        <div class="data"><span class="title">Successful Launch:</span><span class="value">{{item.launch_success}}</span></div>

        <div v-if="item.rocket.first_stage.cores[0].land_success == true || item.rocket.first_stage.cores[0].land_success == false" class="data"><span class="title">Successful Landing:</span><span class="value">{{item.rocket.first_stage.cores[0].land_success}}</span></div>
         <div v-else class="data"><span class="title">Successful Landing:</span><span class="value">NA</span></div>
        
    </div>
</template>
<script>
export default {
    props:["item"]

}
</script>
<style>
#root{
    margin: auto;
    width: 100%;
}
#img_div{
    background: #f2f2f2;
    width: 83%;
    margin: auto;
    border-radius: 8px;
    margin-top: 15px ;
}
#space_image{
    width: 100%;
}
.title{
    font-size: 12px;
    font-weight: bold;
    float: left;
    padding-left: 20px;
}
.value{
    font-size: 12px;
    float: left;
    color: #6772A4;
}
#name{
    padding-left: 20px;
    font-size: 13px;
    font-weight: bolder;
    color: #6772A4;
}
.data{
    display: inline-block;
    width: 100%;
}
@media (min-width: 700px) and (max-width: 1024px){
    #container{
        width: 67%;
        float: right;
        margin: 10px;
    }
}
@media (min-width: 1024px){
    #img_div{
        background: #f2f2f2;
        padding: 30px;
        width: 110px;
        margin: auto;
    }
    #space_image{
        width: 100px;
        height: 100px;
    }
    .title{
        padding-left: 10px;
    }
    #name{
        padding-left: 10px;
        font-size: 13px;
    }
}
</style>