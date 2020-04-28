<template>
  <div class="player">
	<div>
		<img id="poster" :src="posterSong">
		<p id="title">{{titleSong}}</p>
	</div>
	<div style="display: flex;margin-top: 12%;">
		<div class="control">
			<i class="fas fa-backward" @click="changeSong(indexSong-1)"></i>
			<i :class="classPlayOrPause" @click="playOrPause()" ref="playButton"></i>
			<i class="fas fa-forward" @click="changeSong(indexSong+1)"></i>
		</div>
	</div>
	<div class="playBar">
		<input type="range" :value="Math.floor(valueRange)" :max="maxRange" id="range" @mousedown="song.pause()">
	</div>
	<div style="width: 60%;margin-top: 3%">
		<p>{{time}}</p>
	</div>
  </div>
</template>

<script>
export default {
	name: 'Player',
	mounted(){
		this.titleSong = this.songs[this.indexSong].title;
		this.linkSong = this.songs[this.indexSong].link;
		this.posterSong = this.songs[this.indexSong].poster;
		this.song.src = this.songs[this.indexSong].link;
		var range = document.getElementById('range');
		// update play bar 
		this.song.addEventListener('timeupdate',()=>{
			var position = this.song.currentTime / this.song.duration;
			range.value = position*100;
			range.style.background 
				= "linear-gradient(to right,#ebebeb " + range.value + "%, #333 "+ range.value + "%)";
			this.maxRange = 100;
			this.valueRange = this.song.currentTime / this.song.duration;
			if(position==1){
				this.changeSong(this.indexSong+1);
			}
		})
		// edit song'currentTime
		range.addEventListener('change',()=>{
			this.song.currentTime = range.value*this.song.duration/100;
		});

		range.addEventListener('mouseup',(e)=>{
			range.value = (e.layerX/range.offsetWidth)*100;
			this.playOrPause();
		});
	},
	data () {
		return{
			song: new Audio(),
			indexSong: 0,
			titleSong: "",
			linkSong: "",
			posterSong: "",
			songs: [
				{id:"1",title:"Laxed",link:require("../assets/mp3s/laxed.mp3"),poster:require("../assets/images/laxed.png")},
				{id:"2",title:"Balada",link:require("../assets/mp3s/balada.mp3"),poster:require("../assets/images/balada.jpg")},
				{id:"3",title:"Ai seu chi pego",link:require("../assets/mp3s/bai2.mp3"),poster:require("../assets/images/aiseuchitego.jpg")},
			],
			classPlayOrPause: "fas fa-play",
			valueRange: 0,
			maxRange: 100,
			time: "00:00/00:00"
		}
	},
	props: {
		
	},
	methods: {
		// play or pause
		playOrPause: function(){
			if(this.song.paused){
				this.song.play();
				this.classPlayOrPause = "fas fa-pause";
			}
			else{
				this.song.pause();
				this.classPlayOrPause = "fas fa-play";
			}
		},
		// check change song
		changeSong: function(indexSong){
			this.maxRange = 0;
			this.valueRange = 0;
			if(indexSong<0)
				this.indexSong=this.songs.length-1;
			else
				if(indexSong==this.songs.length)
					this.indexSong=0;
				else
					this.indexSong = indexSong;
		},

		convertTime: function(time){
			var min = Math.floor(time/60);
			var sec = time%60;
			min = (min<10) ? '0' + min : min;
			sec = (sec<10) ? '0' + sec : sec;
			var result = min + ":" + sec;
			return result;
		}
	},
	watch: {
		// do when change song
		indexSong: function(){
			this.song.src = this.songs[this.indexSong].link;
			this.posterSong = this.songs[this.indexSong].poster;
			this.titleSong = this.songs[this.indexSong].title;
			this.time = "00:00 / " + this.convertTime(Math.round(this.songs[this.indexSong].duration));
			this.song.play();
			this.classPlayOrPause = "fas fa-pause";
		},

		valueRange: function(){
			this.time = this.convertTime(Math.floor(this.song.currentTime)) + " / " + this.convertTime(Math.round(this.song.duration));
		}
	}
}
</script>

<style scoped>
*{
	margin: 0 auto;
	padding: 0;
}

#player{
	display: block;
	margin-top: 75px;
	height: 375px;
	width: 30%;
	border-radius: 10px;
	background-image: linear-gradient(to right,#D585FF,#00FFEE);
	text-align: center;
}

#poster{
	margin-top: 10%;
	height: 175px;
	width: 175px;
	border-radius: 50%;
	animation: rotation 15s linear infinite;
}

@keyframes rotation {
	from {transform: rotate(0);}
	to {transform: rotate(360deg);}
}

.control{
	width: 80%;
	display: flex;
}

.control i{
	font-size: 25px;
	color: #333;
	cursor: pointer;
}

.playBar{
	position: relative;
	width: 60%;
	height: 5px;
	display: flex;
	margin-top: 5%;
}

.playBar #range{
	-webkit-appearance: none;
	height: 5px;
	width: 100%;
	background: linear-gradient(to right,#ebebeb 0%, #333 0%);
	outline: none;
	cursor: pointer;
}

.playBar #range::-webkit-slider-thumb{
	-webkit-appearance: none;
	border-radius: 50%;
	height: 15px;
	width: 15px;
	background: transparent;
	cursor: pointer;
}
</style>
