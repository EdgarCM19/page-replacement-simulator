<template>
	<div>
		<!-- <transition
			enter-active-class="transition ease-out duration-300"
			enter-from-class="opacity-0"
			enter-to-class="opacity-100"
			leave-active-class="transition ease-in duration-200"
			leave-from-class="opacity-100"
			leave-to-class="opacity-0"
		> -->
		<div v-show="showing" class="modal">
			<div class="modal__container">
				<div class="modal__header" aria-hidden="true">
					<button
						class="btn expand round btn-close"
						aria-label="close"
						@click.prevent="close"
						:style="{ color: settings.getters.getColor() }"
					>
					<Icon
						:icon="'close'"
						:height="settings.getters.getFontSize()"
						:width="settings.getters.getFontSize()"
					/>
					</button>
				</div>
				<div class="modal__content">
					<slot></slot>
				</div>
			</div>
		</div>
		<!-- </transition> -->

	</div>
</template>

<script lang="ts">
import { defineComponent, inject } from 'vue';
import Icon from '@/components/Icon/Icon.vue';

export default defineComponent({
	components: {
		Icon,
	},
	props: {
		showing: {
			type: Boolean,
			required: true,
			default: false,
		}
	},
	setup(_, { emit }) {

		const settings = inject('settings');

		const close = () => {
			console.log('Modal closed');
			emit('update:showing', false);
			emit('close');
			console.log('Que pasa')
		}
		return {
			settings,
			close
		}
	}
})
</script>

<style lang="scss">
@import './modal';
</style>