<template>
    <div class="input">
        
         <tabs class="card">
				<tab name="Wysiwyg" key="bb">
                     <div class="pell" :id="id" @keydown.ctrl.66.prevent="bold" @keydown.ctrl.73.prevent="italic" @keydown.ctrl.85.prevent="underline" @keydown.ctrl.83.prevent="strikethrough"></div>
				</tab>
                <tab name="Raw" key="dd">
                    <text />
				</tab>
        </tabs>
        <label>{{ label }}</label>

        <label class="error">{{ error }}</label>
    </div>
</template>

<script>

    import { init } from "pell";
    import Turndown from "turndown"
    import showdown from "showdown";
    import Text from './text';

    let converter = new showdown.Converter({
        tables: true
    });

    let turndown = new Turndown();

    export default {
        name: "markdown",
        props: {
            value: {
                type: String
            },
            label: {
                type: String
            },
            error: {
                type: String
            }
        },

        data() {
            return {
                id: `editor${Math.random()}`,
                textareaID: `textarea${Math.random()}`,
                markdown: null
            }
        },

        methods: {
            bold() {
                document.execCommand("bold");
            },

            italic() {
                document.execCommand("italic");
            },

            underline() {
                document.execCommand("underline");
            },

            strikethrough() {
                document.execCommand("strikethrough");
            }
        },
        
        mounted() {

            const editor = init({
                element: document.getElementById(this.id),
                onChange: html => {
                    this.markdown = turndown.turndown(html);
                    this.$emit("input", this.markdown);
                }
            });
            
            editor.content.innerHTML = converter.makeHtml(this.value || "");

        }
    };
</script>