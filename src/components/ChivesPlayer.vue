<template>
	<div id="dplayer" class="play-root"></div>
    <input type="file" id = "input" accept="*" v-on:change="playSelectedFile"  />
	<input type="file" id = "input_sub" accept="*" v-on:change="add_subtitle"  />

  	<button v-on:click="init_dplayer()">init</button>
<!--  <script></script>-->
</template>

<script>

import Dplayer from 'dplayer';
import shaka from 'shaka-player';
import ASS from 'assjs';

export default {
    name: "ChivesPlayer",
    data() {
        return {
            msg:'hello vue!',
            dplayer:'', 
            fileURL:'',
            shaka_player:'',
        }
    },
    created() {
        console.log('dp_app');
    },
    methods:{
        playSelectedFile(){
            var inputNode=document.getElementById("input");
            var file = inputNode.files[0];
            var fileURL = URL.createObjectURL(file,);
            this.fileURL = fileURL;
            console.log('file selected');
            this.dplayer.switchVideo({url: fileURL}, {});
        },
        init_dplayer(){
            const dp = new Dplayer({
                container:document.getElementById('dplayer'),
                video:{
                  	url:'../../tests/files/test1.mkv',
                  	type:'shakaDash',
                  	customType: {
                    	shakaDash: function (video, player) {
                      		var src = video.src;
                      		var test_src = 'https://cdn.plyr.io/static/demo/View_From_A_Blue_Moon_Trailer-576p.mp4';
                            var playerShaka = new shaka.Player(video);
                            playerShaka.load(src);
                            console.log('shaka player confirm');
                    }
                  }
                },
                subtitle: {
                    url: '../../tests/files/Justice.League.Snyders.Cut.2021.WEBRip.PD.EN&amp;CHS.vtt',
                    type: 'webvtt',
                    fontSize: '25px',
                    bottom: '10%',
                    color: '#b7daff',
                },
                danmaku:{
                    id: 'vid-example',
                    user:1000,
                    token:'token-example',
                    api: 'http://127.0.0.1:8080/',
                    addition: ['http://161.35.234.230:1207/v3/bilibili?aid=80266688&cid=137358410']
                },
                screenshot:true,
            })
            this.dplayer = dp;
        },
        add_subtitle(){
			function readFile(event) {
  				console.log(event.target.result);
			}
            var inputNode=document.getElementById("input_sub");
            var file = inputNode.files[0];
			console.log('file reading');
			var reader = new FileReader();
			reader.addEventListener('load', readFile)
			reader.readAsText(file);
			console.log(reader.result);
			console.log('file red');
        },
    }
}
</script>
<style scoped>
</style>
