<template lang="slm">
div.col.gap-10
	promise-form.col.gap-5 :action="execute" ref="ref_form"
		.row.align-center.gap-10
			code git 
			input.command.flex-1 v-model="command" ref="command_input_ref"
		.input-controls.justify-flex-end.align-center.gap-10
			div.warn v-if="text_changed"
				details
					summary Edited. Be careful!
					| Editing this field can be dangerous, as it is executed without escaping. If you do not know what you are doing, please click Reset.
			button.btn.btn-2.gap-3 type="button" v-if="text_changed" @click="reset_command()"
				i.codicon.codicon-discard
				| Reset
			div v-if="is_saved && ! has_unsaved_changes"
				| Saved
			button.btn.btn-2.gap-3 type="button" v-if="has_unsaved_changes" @click="save()"
				i.codicon.codicon-save
				| Save
		.param v-for="(param, i) in params"
			label.row.align-center.gap-5
				| Param \${{ i+1 }}
				input.flex-1 v-model="params[i]" onfocus="select()" ref="params_input_refs"
		div
			button.btn.gap-3
				i.codicon.codicon-check
				| Execute
	.error-response.padding-l v-if="error"
		| Command failed: 
		| {{ error }}
	.success-response.padding-l v-if="data"
		| Successful result:<br>
		| {{ data }}
	div v-if="options.length"
		div Common options
		ul.options
			li v-for="option of options" :class="{changed: option.active !== option.default_active}"
				label.row.align-center.gap-5
					input type="checkbox" v-model="option.active" :disabled="text_changed"
					| {{ option.value }}
</template>

<script lang="coffee" src="./GitInput.coffee"></script>

<style lang="stylus">
.options
	.changed
		border-left 2px solid #bc9550 // like vscode settings ui
.error-response, .success-response
	white-space pre-wrap
.param
	white-space pre
.error-response
	color #e53c3c
</style>