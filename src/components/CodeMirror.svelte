<script context="module">
  const is_browser = typeof window !== "undefined";

  let codemirror_promise;
  let _CodeMirror;

  if (is_browser) {
    // TODO check if import is still async
    codemirror_promise = import("codemirror");

    codemirror_promise.then(mod => {
      _CodeMirror = mod.default;
    });
  }

</script>

<script>

  import { onMount, createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  const refs = {}

  onMount(() => {
    if (_CodeMirror) {
      CodeMirror = _CodeMirror;
      createEditor(mode || "svelte", theme).then(() => {
        if (editor) editor.setValue(value || "");
      });
    } else {
      codemirror_promise.then(async mod => {
        CodeMirror = mod.default;
        await createEditor(mode || "svelte", theme);
        if (editor) editor.setValue(value || "");
      });
    }

    return () => {
      destroyed = true;
      if (editor) editor.toTextArea();
    };
  });

  async function createEditor(mode, theme) {
    if (destroyed || !CodeMirror) return;

    if (editor) editor.toTextArea();

    // console.log("createEditor:", theme);

    const opts = {
      lineNumbers,
      lineWrapping: true,
      indentWithTabs: true,
      indentUnit: 2,
      tabSize: 2,
      value: "",
      mode: modes[mode] || {
        name: mode
      },
      readOnly: readonly,
      autoCloseBrackets: true,
      autoCloseTags: true,
      extraKeys: {}
    };

    if(theme !== undefined) 
      opts.theme = theme;
    

    if (!tab)
      opts.extraKeys = {
        Tab: tab,
        "Shift-Tab": tab,
      };

    if(cmdEnter)
      opts.extraKeys["Cmd-Enter"] = (cmdEnter);

    if(ctrlEnter)
      opts.extraKeys["Ctrl-Enter"] = (ctrlEnter);

    if(shiftEnter)
      opts.extraKeys["Shift-Enter"] = (shiftEnter);

    if(cmdPeriod)
      opts.extraKeys["Cmd-."] = (cmdPeriod);

    if(ctrlPeriod)
      opts.extraKeys["Ctrl-."] = (ctrlPeriod);

    if(cmdHiffen)
      opts.extraKeys["Cmd--"] = (cmdHiffen);

    if(ctrlHiffen) 
      opts.extraKeys["Ctrl--"] = (ctrlHiffen);
    
    if(cmdEqual)
      opts.extraKeys["Cmd-="] = (cmdEqual);

    if(ctrlEqual)
      opts.extraKeys["Cmd-="] = (ctrlEqual);

    if(cmdCloseSquareBracket)
      opts.extraKeys["Cmd-]"] = (cmdCloseSquareBracket);

    if(cmdOpenSquareBracket)
      opts.extraKeys["Cmd-["] = (cmdOpenSquareBracket);      

    if(ctrlCloseSquareBracket)
      opts.extraKeys["Ctrl-]"] = (ctrlCloseSquareBracket);

    if(ctrlOpenSquareBracket)
      opts.extraKeys["Ctrl-["] = (ctrlOpenSquareBracket);      

    if(cmdForwardSlash)
      opts.extraKeys["Cmd-/"] = () => editor.execCommand('toggleComment');

    if(ctrlForwardSlash)
      opts.extraKeys["Ctrl-/"] = () => editor.execCommand('toggleComment');

    // if(ctrlForwardSlash)
    //   opts.extraKeys["Ctrl-/"] = (ctrlForwardSlash);      

    // if(cmdEnter && !opts.extraKeys["Cmd-Enter"])
    //   opts.extraKeys["Cmd-Enter"] = (cmdEnter);


    // Creating a text editor is a lot of work, so we yield
    // the main thread for a moment. This helps reduce jank
    if (first) await sleep(50);

    if (destroyed) return;

    editor = CodeMirror.fromTextArea(refs.editor, opts);

    editor.on("change", instance => {
      if (!updating_externally) {
        const value = instance.getValue();
        dispatch("change", { value });
      }
    });

    if (first) await sleep(50);
    editor.refresh();

    first = false;
  }  

</script>

<style>
  h1 {
    color: purple;
  }
</style>

<textarea {value}
          tabindex="0" 
          bind:this={refs.editor} 
          readonly />

{#if !CodeMirror}
  <pre>{value}</pre>
{/if}
