<template>
	<div class="icon-voice">
		<icon-svg :name="`voice${index}`" />
	</div>
</template>

<script>
export default {
	props: {
		play: Boolean
	},

	data() {
		return {
			timer: null,
			index: ""
		};
	},

	watch: {
		play(val) {
			clearInterval(this.timer);

			console.log(val)

			if (val) {
				this.index = 1;

				this.timer = setInterval(() => {
					if (this.index == 1) {
						this.index = "";
					} else {
						this.index += 1;
					}
				}, 500);
			} else {
				this.index = "";
			}
		}
	}
};
</script>

<style lang="scss" scoped>
.icon-voice {
	display: inline-block;
	margin-right: 5px;
}
</style>
