# Create Validatable Form

Create a new form with fields with validated inputs.

## Kata Solution

Solution is to:

- init `validation` component on the form element
- add needed validation rules to input `data-validate` attributes

## Create a new form template

`view/base/requirejs-config.js`:

```php
<?php
/** @var \Magento\Framework\View\Element\Template $block */
?>
<form class="form"
      method="post"
      data-hasrequired="<?= $block->escapeHtmlAttr(__('* Required Fields')) ?>"
      data-mage-init='{"validation":{}}'>
    <fieldset class="fieldset">
        <legend class="legend">
            <span><?= $block->escapeHtml(__('Contact Form')) ?></span>
        </legend>
        <br />
        <div class="field name required">
            <label class="label" for="name">
                <span><?= $block->escapeHtml(__('Name')) ?></span>
            </label>
            <div class="control">
                <input name="name"
                       id="name"
                       title="<?= $block->escapeHtmlAttr(__('Name')) ?>"
                       class="input-text"
                       type="text"
                       data-validate="{required:true}"
                />
            </div>
        </div>
        <div class="field email required">
            <label class="label" for="email"><span><?= $block->escapeHtml(__('Email')) ?></span></label>
            <div class="control">
                <input name="email"
                       id="email"
                       title="<?= $block->escapeHtmlAttr(__('Email')) ?>"
                       class="input-text"
                       type="email"
                       data-validate="{required:true, 'validate-email':true, 'validate-trusted-emails': true}"
                />
            </div>
        </div>
        <div class="field comment required">
            <label class="label" for="comment">
                <span><?= $block->escapeHtml(__('What’s on your mind?')) ?></span>
            </label>
            <div class="control">
                <textarea name="comment"
                          id="comment"
                          title="<?= $block->escapeHtmlAttr(__('What’s on your mind?')) ?>"
                          class="input-text"
                          cols="5"
                          rows="3"
                          data-validate="{required:true}">
                </textarea>
            </div>
        </div>
    </fieldset>
    <div class="actions-toolbar">
        <div class="primary">
            <button type="submit" title="<?= $block->escapeHtmlAttr(__('Submit')) ?>" class="action submit primary">
                <span><?= $block->escapeHtml(__('Submit')) ?></span>
            </button>
        </div>
    </div>
</form>
```
