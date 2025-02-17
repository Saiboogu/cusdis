<script>
  import { getContext } from 'svelte'
  import { t } from '../i18n'

  import Reply from './Reply.svelte'
  export let comment
  export let showReplyForm = false
  export let isChild = false

  const { showIndicator } = getContext('attrs')

</script>

<div
  class="my-4"
  class:pl-4={isChild}
  class:border-l-2={isChild}
  class:border-color-gray-200={isChild}
  class:cusdis-indicator={showIndicator}
>
  <div class="cmt-author-line flex items-center">
    <div class="cmt-author mr-2">
      {comment.moderator && comment.moderator.displayName ? comment.moderator.displayName : comment.by_nickname}
    </div>

    {#if comment.moderatorId}
      <div class="cmt-mod mr-2">
        <span>{t('mod_badge')}</span>
      </div>
    {/if}
    <div class="cmt-date">
      {comment.parsedCreatedAt}
    </div>
  </div>



  <div class="cmt-message my-2">
    {@html comment.parsedContent}
  </div>

  {#if comment.replies.data.length > 0}
    {#each comment.replies.data as child (child.id)}
      <svelte:self isChild={true} comment={child} />
    {/each}
  {/if}

  <div>
    <button
      class="cmt-reply-btn"
      type="button"
      on:click={(_) => {
        showReplyForm = !showReplyForm
      }}>{t('reply_btn')}</button
    >
  </div>


  {#if showReplyForm}
    <div class="cmt-reply-form mt-4 pl-4">
      <Reply
        parentId={comment.id}
        onSuccess={() => {
          showReplyForm = false
        }}
      />
    </div>
  {/if}


</div>
