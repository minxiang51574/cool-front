<template>
	<div class="login-captcha" @click="refresh">
		<div v-if="svg" class="svg" v-html="svg"></div>
		<img v-else class="base64" :src="base64" alt="" />
	</div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import { ElMessage } from "element-plus";
import { useCool } from "/@/core";

export default defineComponent({
	emits: ["update:modelValue", "change"],

	setup(_, { emit }) {
		const { service } = useCool();

		const base64 = ref<string>("");
		const svg = ref<string>("");

		const refresh = () => {
			service.base.open
				.captcha({
					height: 36,
					width: 110
				})
				.then(({ captchaId, data }: any) => {
					if (data.includes(";base64,")) {
						base64.value = data;
					} else {
						svg.value = data;
					}

					emit("update:modelValue", captchaId);
					emit("change", {
						base64,
						svg,
						captchaId
					});
				})
				.catch((err: string) => {
					ElMessage.error(err);
				});
		};

		refresh();

		return {
			base64,
			svg,
			refresh
		};
	}
});
</script>

<style lang="scss" scoped>
.login-captcha {
	height: 36px;
	cursor: pointer;

	.svg {
		height: 100%;
	}

	.base64 {
		height: 100%;
	}
}
</style>
