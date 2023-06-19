<script>
  import { Textarea, Label } from 'flowbite-svelte';
  import { latToOsm } from '$lib/transliteration';
  import InlineSvg from 'svelte-inline-svg';
  import clipboardIcon from '../assets/clipboard.svg';

  let inputStr = '';
  let outputStr = '';

  function transliterate() {
    outputStr = inputStr.toLowerCase();
    let result = '';

    for (let i = 0; i < outputStr.length; i++) {
      const char = outputStr[i];
//@ts-ignore  

      const replacement = latToOsm[char] || char;
      result += replacement;
    }

    outputStr = result.replace(/\'/g, '𐒀');
  }

function copyToClipboard() {
  const el = document.createElement('textarea');
  el.value = outputStr;
  document.body.appendChild(el);
  el.select();
  document.execCommand('copy');
  document.body.removeChild(el);
}



</script>

<div>
  <Label for="input-textarea" class="mb-2">Your message</Label>
  <Textarea id="input-textarea" placeholder="Your message" rows="4" name="message" bind:value="{inputStr}" on:input="{transliterate}" />
</div>

<!-- <div> -->
<!--   <Label for="output-textarea" class="mb-2">Transliterated message</Label> -->
<!--   <Textarea id="output-textarea" placeholder="Transliterated message" rows="4" name="transliteratedMessage" readonly bind:value="{outputStr}" /> -->
<!-- </div> -->


<!-- <div> -->
<!--   <Label for="output-textarea" class="mb-2">Transliterated message</Label> -->
<!--   <div class="flex items-center"> -->
<!--     <Textarea id="output-textarea" placeholder="Transliterated message" rows="4" name="transliteratedMessage" readonly bind:value="{outputStr}" /> -->
<!--     <button class="ml-2" on:click="{copyToClipboard}" title="Copy to clipboard"> -->
<!--       <Icon icon="{clipboard}" /> -->
<!--     </button> -->
<!--   </div> -->
<!-- </div> -->


<div class="flex items-center">
  <Textarea id="output-textarea" placeholder="Transliterated message" rows="4" name="transliteratedMessage" readonly bind:value="{outputStr}" />
  <button class="ml-2" on:click="{copyToClipboard}" title="Copy to clipboard">
    <InlineSvg src="{clipboardIcon}" class="w-4 h-4" />
  </button>
</div>
