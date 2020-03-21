<script context="module">

  import {EditorView} from "@codemirror/next/view"
  import {EditorState} from "@codemirror/next/state"
  // import {CodeMirror} from "@codemirror" 

  const is_browser = typeof window !== "undefined"

  let codemirror_promise
  let _CodeMirror

  // if (is_browser) {

    // _CodeMirror = new EditorView({state: EditorState.create({doc: "this is a codemirror instance"})})
    // _CodeMirror = mod.default
    // document.body.appendChild(myView.dom)

    // TODO check if import is still async
    // codemirror_promise = import("codemirror")

  //   codemirror_promise.then(mod => {
  //     _CodeMirror = mod.default
  //   });
  // }

</script>

<script>

  import { onMount, createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  let CodeMirror;
  export let value;
  const refs = {};

  // let {                       
  //   EditorState,                 
  //   EditorView,
  //   keymap,                      
  //   history,                     
  //   redo,
  //   redoSelection,               
  //   undo,
  //   undoSelection,
  //   lineNumbers,                 
  //   baseKeymap,
  //   indentSelection,             
  //   legacyMode,
  //   legacyModes: { javascript },
  //   matchBrackets,
  //   specialChars,
  //   multipleSelections           
  // } = CodeMirror;


  onMount(() => {

    let myView = new EditorView({state: EditorState.create({doc: "this is a codemirror instance"})})
    document.body.appendChild(myView.dom)

    // if (_CodeMirror) {
    //   CodeMirror = _CodeMirror;
      // createEditor(mode || "svelte", theme).then(() => {
      //   if (editor) editor.setValue(value || "");
      // });
    // } else {
    //   codemirror_promise.then(async mod => {
    //     CodeMirror = mod.default;
        // await createEditor(mode || "svelte", theme);
        // if (editor) editor.setValue(value || "");
    //   });
    // }

    // // return () => {
    //   destroyed = true;
    //   if (editor) editor.toTextArea();
    // };

  });

  // async function createEditor(mode, theme) {
  // }  

</script>

<style>

</style>

<!-- <textarea {value}
          tabindex="0" 
          bind:this={refs.editor} 
          readonly /> -->

<!-- {#if !CodeMirror}
  <pre>{value}</pre>
{/if} -->
