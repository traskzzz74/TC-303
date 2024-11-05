<template>
  <div class="comp-page-container">
    <section>
      <h1>Button 按钮</h1>
      <div class="wrap">
        <div class="item">
          <div class="head"><h3>button-base</h3></div>

          <div class="item-set">
            <div class="item-set-det">
              <Button size="sm" type="primary">Button</Button>
              <p>primary_sm</p>
            </div>
            <div class="item-set-det">
              <Button size="md" type="primary">Button</Button>
              <p>primary_md</p>
            </div>
            <div class="item-set-det">
              <Button size="lg" type="primary" icon="fill-sparkle">Button</Button>
              <p>primary_lg</p>
            </div>
            <div class="item-set-det">
              <Button size="lgx" type="primary" icon="fill-sparkle">Button</Button>
              <p>primary_lgx</p>
            </div>
          </div>

          <div class="item-set">
            <div class="item-set-det">
              <Button size="sm" type="primary" status="isDisabled">Button</Button>
              <p>--disabled</p>
            </div>
            <div class="item-set-det">
              <Button size="md" type="primary" status="isDisabled">Button</Button>
              <p>--disabled</p>
            </div>
            <div class="item-set-det">
              <Button size="lg" type="primary" status="isDisabled" icon="fill-sparkle">Button</Button>
              <p>--disabled</p>
            </div>
            <div class="item-set-det">
              <Button size="lgx" type="primary" status="isDisabled" icon="fill-sparkle">Button</Button>
              <p>--disabled</p>
            </div>
          </div>

          <div class="item-set">
            <div class="item-set-det">
              <Button size="sm" type="primary" status="isActived">Button</Button>
              <p>--actived</p>
            </div>
            <div class="item-set-det">
              <Button size="md" type="primary" status="isActived">Button</Button>
              <p>--actived</p>
            </div>
            <div class="item-set-det">
              <Button size="lg" type="primary" status="isActived" icon="fill-sparkle">Button</Button>
              <p>--actived</p>
            </div>
            <div class="item-set-det">
              <Button size="lgx" type="primary" status="isActived" icon="fill-sparkle">Button</Button>
              <p>--actived</p>
            </div>
          </div>
          
          <div class="item-set">
            <div class="item-set-det">
              <Button size="sm" type="secondary">Button</Button>
              <p>secondary_sm_disabled</p>
            </div>
            <div class="item-set-det">
              <Button size="md" type="secondary">Button</Button>
              <p>secondary_md</p>
            </div>
            <div class="item-set-det">
              <Button size="lg" type="secondary" icon="fill-sparkle">Button</Button>
              <p>secondary_lg</p>
            </div>
            <div class="item-set-det">
              <Button size="lgx" type="secondary" icon="fill-sparkle">Button</Button>
              <p>secondary_lgx</p>
            </div>
          </div>

          <div class="item-set">
            <div class="item-set-det">
              <Button size="sm" type="tertiary">Button</Button>
              <p>tertiary_sm</p>
            </div>
            <div class="item-set-det">
              <Button size="md" type="tertiary">Button</Button>
              <p>tertiary_md</p>
            </div>
            <div class="item-set-det">
              <Button size="lg" type="tertiary" icon="fill-sparkle">Button</Button>
              <p>tertiary_lg</p>
            </div>
            <div class="item-set-det">
              <Button size="lgx" type="tertiary" icon="fill-sparkle">Button</Button>
              <p>tertiary_lgx</p>
            </div>
          </div>

        </div>
      </div>
    </section>
    <section>
      <h1>Selection 选项</h1>
    </section>

  </div>
</template>

<script>
import Button from './components/button.vue';

export default {
  components: {
    Button,
  },
};
</script>

<style>
</style>