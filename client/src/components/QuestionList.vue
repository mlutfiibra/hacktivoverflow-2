<template>
    <div>
        <ul class="list-group mt-4">
            <li 
                class="list-group-item"
                v-for="question in questions"
                :key="question._id"
            >
            <div class="response">
                <div class="response__answer">
                    <div>{{question.answerId.length}}</div>
                    <div class="desc">Answer</div>
                </div>
                <div class="response__answer">
                    <div>
                        {{question.upvote ? question.upvote.length : 0}}
                    </div>
                    <span class="desc">Upvote</span>
                </div>
                <div class="response__answer">
                    <div>
                        {{question.downvote ? question.downvote.length : 0}}
                    </div>
                    <span class="desc">Downvote</span>
                </div>
            </div>
            <div class="title">
                <div class="content">
                    <router-link :to="`/question/${question._id}`" >
                        <div class="header">{{question.title}}</div>
                    </router-link>
                    <div class="manipulate" v-if="manipulate">
                        <div class="edit" @click="editQuestion(question._id)">Edit</div>
                        <div class="delete" @click="deleteQuestion(question._id)">Delete</div>
                    </div>
                </div>
                <div class="tags-container">
                    <ul class="tags">
                        <li  v-for="(tag, index) in question.tags" :key="index">{{tag}}</li>
                    </ul>
                    <div class="author">
                        by {{question.userId.username}}
                    </div>
                </div>
            </div>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    props: ['questions', 'manipulate'],
    methods: {
        editQuestion(id) {
            this.$router.push(`/question/edit/${id}`)
        },
        deleteQuestion(id) {
            this.$alertify.confirm("Are you sure?",
                () => {
                    this.$store.dispatch('deleteQuestion', id)
                    .then(() => {
                        if(!this.$store.getters.error){
                            this.$alertify.success('Question deleted');
                        }
                    })
                },
                () => {
                    this.$alertify.error('Failed, please check your internet connection or try again');
                }
            );
        }
    },
}
</script>

<style scope>
.response {
    display: flex;
    width: 173px;
    justify-content: space-between;
}
.title {
    text-align: left;
    width: 29em;
    margin-left: 29px;
}
.header {
    font-size: 1.1em;
    font-weight: 600;
    color: #616161;
}
.list-group li {
    display: flex;
    justify-content: space-between;
}
.response__answer {
    color: #616161;

    text-align: center;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    height: 43px;
}
.response__answer > .desc {
    font-size: 12px;
}
.tags-container {
    display: flex;
    justify-content: space-between;
    width: 29em;
}
.tags {
    padding: 0px;
    text-align: left;
}
.tags li {
    display: inline-block;

    background-color: #eee;
    border-radius: 3px;
    padding: 0px 9px;
    color: #616161;
    font-weight: 500;
    display: inline-block;
    font-size: 13px;
    line-height: 17px;
}
.tags li:not(:first-child) {
    margin-left: 7px;
}
.author {
    font-size: 13px;
    color: #616161;
}
.content {
    display: flex;
    justify-content: space-between;
    margin-bottom: .7em;
}
.manipulate {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    width: 7.5em;
    -webkit-box-pack: justify;
    -ms-flex-pack: justify;
    justify-content: space-between;
    font-size: 15px;
    flex-direction: column;
    text-align: right;
    height: 1em;
}
.manipulate .edit {
    color: #8f97ff;
}
.manipulate .edit:hover {
    color: #5c64c0;
    cursor: pointer;
}
.manipulate .delete {
    color: #f85d5d;
}
.manipulate .delete:hover {
    color: #ed1616;
    cursor: pointer;
}
</style>
