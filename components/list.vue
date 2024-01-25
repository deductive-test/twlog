<template>
	<div>
		<p v-show="false">{{data}}</p>

		<p v-if="loading">loading: {{data.param.value}}</p>
		<p v-else class="small right">loaded: {{data.param.value}} [{{tw.length}}]</p>

		<div v-for="(t) in tw" :key="t.id">
			<p class="small">{{t.created_at_str}}</p>
			<p class="pre">{{t.full_text}}</p>
			<p class="small"><a :href="`${common.URL.BASE}view/detail.html?target=${data.param.value}&id=${t.id}`" target="_blank">#{{t.id}}</a></p>
			<hr>
		</div>
		
	</div>
</template>

<script>
module.exports = {
	mixins:[myMixins],
	name: 'List',
	components: {
	},
	props: {
		data: {
			type: Object,
			default: () => {},
		},
	},
	data () {
		return {
			loading:true,
			tw:[],
		}
	},
	mounted () {
		this.load()
	}, // mounted
	methods: {
		load(){
			this.zjax({
				type: 'GET',
				url: `${this.common.URL.TWDATA}${this.data.param.value}/data/tweet${this.data.param.data.isTweet === true ? '' : 's'}.js`,
				dataType: 'text',
				},
				done=>{
					let txt = done.replace(`window.YTD.tweet${this.data.param.data.isTweet ? '' : 's'}.part0 = `, '')
					let row = JSON.parse(txt)

					for (let i = 0; i < row.length; i++) {
						const rowTw = this.data.param.data.isChildTweet ? row[i].tweet : row[i]

						if (!rowTw.full_text.includes(this.data.input.keyword)) continue
						

						let add = {}
						add.id = rowTw.id
						add.full_text = rowTw.full_text
						add.created_at = new Date(rowTw.created_at)
						add.created_at_str = this.FormatDate(add.created_at, '/', true, true)

						this.tw.push(add)
					} // for [row]
				},
				fail=>{},
				always=>{
					this.loading = false
				})
		}, // load
	}, // methods
}
</script>

<style scoped>
p.pre {
	word-break: break-all;
	white-space: pre-wrap;
}
p.small {
	font-size: small;
}
p.right {
	text-align: end;
}
</style>
