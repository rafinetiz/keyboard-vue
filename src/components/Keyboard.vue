<script setup lang="ts">
import { onMounted, ref } from 'vue';
import KeyboardKey from './KeyboardKey.vue';
import KeyboardKeyDivider from './KeyboardKeyDivider.vue';

const keymap = ref<Record<string, {
  state: 0 | 1,
  pressed: boolean;
}>>({});

const keyup_handler: Record<string, ReturnType<typeof setTimeout>> = {};

onMounted(() => {
  if (window) {
    window.addEventListener('keydown', (event) => {
      event.preventDefault();

      console.log(event.code, '=>', event.key);

      if (!(event.code in keymap.value)) {
        keymap.value = {
          [event.code]: {
            state: 0,
            pressed: true
          },
          ...keymap.value
        }
      } else {
        if (keymap.value[event.code].pressed) {
          return
        }

        keymap.value[event.code].pressed = true;
      }

      // di beberapa OS meta key tidak akan mengirim keyup event dikarenakan browser terinterupsi oleh OS itu sendiri
      if (event.code === 'MetaLeft') {
        if ((event.code in keyup_handler) && keyup_handler[event.code]) {
          clearTimeout(keyup_handler[event.code]);
        }

        keyup_handler[event.code] = setTimeout(() => {
          keymap.value[event.code].pressed = false;
        }, 100);
      }
    });

    window.addEventListener('keyup', (event) => {
      event.preventDefault();
      if (!(event.code in keymap.value)) {
        // mengatasi beberapa key tidak mengirim keydown (contoh: PrintScreen key)
        keymap.value = {
          [event.code]: {
            state: 0,
            pressed: true,
          },
          ...keymap.value
        }
      }

      keymap.value[event.code].state = 1;
      keymap.value[event.code].pressed = false;
    });
  }
});
</script>

<template>
  <div class="keyboard-container">
    <!-- main keyboard area -->
    <div class="col left">
      <div class="row">
        <KeyboardKey :keymap code="Escape" label="ESC" />

        <KeyboardKeyDivider grow="1" />

        <KeyboardKey :keymap code="F1" label="F1" />
        <KeyboardKey :keymap code="F2" label="F2" />
        <KeyboardKey :keymap code="F3" label="F3" />
        <KeyboardKey :keymap code="F4" label="F4" />

        <KeyboardKeyDivider grow="1" />

        <KeyboardKey :keymap code="F5" label="F5" />
        <KeyboardKey :keymap code="F6" label="F6" />
        <KeyboardKey :keymap code="F7" label="F7" />
        <KeyboardKey :keymap code="F8" label="F8" />

        <KeyboardKeyDivider grow="1" />

        <KeyboardKey :keymap code="F9" label="F9" />
        <KeyboardKey :keymap code="F10" label="F10" />
        <KeyboardKey :keymap code="F11" label="F11" />
        <KeyboardKey :keymap code="F12" label="F12" />
      </div>

      <div class="row">
        <KeyboardKey :keymap code="Backquote" label="~" />
        <KeyboardKey :keymap code="Digit1" label="1" />
        <KeyboardKey :keymap code="Digit2" label="2" />
        <KeyboardKey :keymap code="Digit3" label="3" />
        <KeyboardKey :keymap code="Digit4" label="4" />
        <KeyboardKey :keymap code="Digit5" label="5" />
        <KeyboardKey :keymap code="Digit6" label="6" />
        <KeyboardKey :keymap code="Digit7" label="7" />
        <KeyboardKey :keymap code="Digit8" label="8" />
        <KeyboardKey :keymap code="Digit9" label="9" />
        <KeyboardKey :keymap code="Digit0" label="0" />
        <KeyboardKey :keymap code="Minus" label="-" />
        <KeyboardKey :keymap code="Equal" label="=" />
        <KeyboardKey :keymap code="Backspace" label="Backspace" width="100px" grow="1" />
      </div>

      <div class="row">
        <KeyboardKey :keymap code="Tab" label="Tab" grow="2" />
        <KeyboardKey :keymap code="KeyQ" label="Q" />
        <KeyboardKey :keymap code="KeyW" label="W" />
        <KeyboardKey :keymap code="KeyE" label="E" />
        <KeyboardKey :keymap code="KeyR" label="R" />
        <KeyboardKey :keymap code="KeyT" label="T" />
        <KeyboardKey :keymap code="KeyY" label="Y" />
        <KeyboardKey :keymap code="KeyU" label="U" />
        <KeyboardKey :keymap code="KeyI" label="I" />
        <KeyboardKey :keymap code="KeyO" label="O" />
        <KeyboardKey :keymap code="KeyP" label="P" />
        <KeyboardKey :keymap code="BracketLeft" label="[" />
        <KeyboardKey :keymap code="BracketRight" label="]" />
        <KeyboardKey :keymap code="Backslash" label="\" grow="1" />
      </div>

      <div class="row">
        <KeyboardKey :keymap code="CapsLock" label="Capslock" grow="1" />
        <KeyboardKey :keymap code="KeyA" label="A" />
        <KeyboardKey :keymap code="KeyS" label="S" />
        <KeyboardKey :keymap code="KeyD" label="D" />
        <KeyboardKey :keymap code="KeyF" label="F" />
        <KeyboardKey :keymap code="KeyG" label="G" />
        <KeyboardKey :keymap code="KeyH" label="H" />
        <KeyboardKey :keymap code="KeyJ" label="J" />
        <KeyboardKey :keymap code="KeyK" label="K" />
        <KeyboardKey :keymap code="KeyL" label="L" />
        <KeyboardKey :keymap code="Semicolon" label=";" />
        <KeyboardKey :keymap code="Quote" label="'" />
        <KeyboardKey :keymap code="Enter" label="Enter" grow="1" />
      </div>

      <div class="row">
        <KeyboardKey :keymap code="ShiftLeft" label="Shift" grow="1" />
        <KeyboardKey :keymap code="KeyZ" label="Z" />
        <KeyboardKey :keymap code="KeyX" label="X" />
        <KeyboardKey :keymap code="KeyC" label="C" />
        <KeyboardKey :keymap code="KeyV" label="V" />
        <KeyboardKey :keymap code="KeyB" label="B" />
        <KeyboardKey :keymap code="KeyN" label="N" />
        <KeyboardKey :keymap code="KeyM" label="M" />
        <KeyboardKey :keymap code="Comma" label="," />
        <KeyboardKey :keymap code="Period" label="." />
        <KeyboardKey :keymap code="Slash" label="/" />
        <KeyboardKey :keymap code="ShiftRight" label="Shift" grow="1" />
      </div>

      <div class="row">
        <KeyboardKey :keymap code="ControlLeft" label="Ctrl" />
        <KeyboardKey :keymap code="MetaLeft" label="Win" />
        <KeyboardKey :keymap code="AltLeft" label="Alt" />
        <KeyboardKey :keymap code="Space" label="Space" grow="1" />
        <KeyboardKey :keymap code="AltRight" label="Alt" />
        <!-- Currently unuseable -->
        <KeyboardKey :keymap code="Function" label="Fn" />
        <KeyboardKey :keymap code="ContextMenu" label="Ctx" />
        <KeyboardKey :keymap code="ControlRight" label="Ctrl" />
      </div>
    </div>
    <!-- arrow area -->
    <div class="col middle">
      <div class="row">
        <KeyboardKey :keymap code="PrintScreen" label="Print Screen" />
        <KeyboardKey :keymap code="ScrollLock" label="Scroll Lock" />
        <KeyboardKey :keymap code="Pause" label="Pause" />
      </div>

      <div class="row">
        <KeyboardKey :keymap code="Insert" label="Insert" />
        <KeyboardKey :keymap code="Home" label="Home" />
        <KeyboardKey :keymap code="PageUp" label="Page Up" />
      </div>

      <div class="row">
        <KeyboardKey :keymap code="Delete" label="Delete" />
        <KeyboardKey :keymap code="End" label="End" />
        <KeyboardKey :keymap code="PageDown" label="Page Down" />
      </div>

      <KeyboardKeyDivider grow="1" />

      <div class="row">
        <KeyboardKeyDivider grow="1" />
        <KeyboardKey :keymap code="ArrowUp" label="&uarr;" />
        <KeyboardKeyDivider grow="1" />
      </div>

      <div class="row">
        <KeyboardKey :keymap code="ArrowLeft" label="&larr;" />
        <KeyboardKey :keymap code="ArrowDown" label="&darr;" />
        <KeyboardKey :keymap code="ArrowRight" label="&rarr;" />
      </div>
    </div>
    <!-- numpad area -->
    <div class="col right">
      <KeyboardKeyDivider grow="1" />
      <div class="row">
        <div class="col">
          <div class="row">
            <KeyboardKey :keymap code="NumLock" label="Num Lock" />
            <KeyboardKey :keymap code="NumpadDivide" label="/" />
            <KeyboardKey :keymap code="NumpadMultiply" label="*" />
          </div>

          <div class="row">
            <KeyboardKey :keymap code="Numpad7" label="7" />
            <KeyboardKey :keymap code="Numpad8" label="8" />
            <KeyboardKey :keymap code="Numpad9" label="9" />
          </div>

          <div class="row">
            <KeyboardKey :keymap code="Numpad4" label="4" />
            <KeyboardKey :keymap code="Numpad5" label="5" />
            <KeyboardKey :keymap code="Numpad6" label="6" />
          </div>
          <div class="row">
            <KeyboardKey :keymap code="Numpad1" label="1" />
            <KeyboardKey :keymap code="Numpad2" label="2" />
            <KeyboardKey :keymap code="Numpad3" label="3" />
          </div>

          <div class="row">
            <KeyboardKey :keymap code="Numpad0" label="0" grow="1" />
            <KeyboardKey :keymap code="NumpadPeriod" label="." />
          </div>
        </div>

        <div class="col">
          <KeyboardKey :keymap code="NumpadSubtract" label="-" />
          <KeyboardKey :keymap code="NumpadAdd" label="+" grow="1" />
          <KeyboardKey :keymap code="NumpadEnter" label="Enter" grow="1" />
        </div>
      </div>

    </div>
  </div>
</template>