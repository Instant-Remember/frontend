<template>
    <div class="comments">
        <div class="line"></div>
        <div class="comment">
            <img src="/src/assets/img/friends/rodion.svg" alt="" class="avatar">
            <div class="comment_username">Родион</div>
            <div class="main_comment">Блин дружище, ты большой молодец, все получится!</div>
        </div>

        <div class="my_comment">
            <input type="text" class="write_comment" placeholder="Напишите свой комментарий">
            <button class="send"><img src="/src/assets/img/send.svg" alt=""></button>
        </div>
    </div>
</template>

<script>
export default {

}
</script>

<style scoped>
.comments {
    width: 724px;
    height: auto;

    display: grid;
    grid-template-columns: 724px;
    grid-template-rows: auto 47px;

    margin-left: 17px;
}

.line{
    margin-left: -17px;
    height: 10px;
    width: 757px;

    grid-row: 1;
    grid-column: 1 / 2;
    background-color: #E8ECF2;
}

.comment {
    grid-row: 1;
    grid-column: 1;

    grid-row: 1;
    display: grid;
    grid-template-columns: 41px 645px;
    grid-template-rows: 40px auto;
    
}

.avatar {
    width: 28px;
    height: 28px;
    grid-row: 1;
    grid-column: 1;
    margin-top: 12px;
    margin-left: 11px;
}

.comment_username {
    grid-row: 1;
    grid-column: 2;

    margin-top: 12px;
    margin-left: 5px;

    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 15px;
    line-height: 18px;
    display: flex;
    align-items: center;

    color: #444444;
}

.main_comment {
    grid-row: 2;
    grid-column: 2;
    display: flex;
    margin-left: 4px;


    font-family: 'Roboto';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 22px;

    color: #444444;
}

.my_comment{
    grid-row: 2;
    grid-column: 1;

    width: 687px;
    display: flex;
    margin-left: 19px;

    margin-top: 5px;
}

.write_comment {
   
   
    display: flex;


    font-family: 'Inter';
    font-style: normal;
    font-weight: 400;
    font-size: 15px;
    line-height: 18px;
    display: flex;
    align-items: center;

    color: #8C8C8C;
}

.send{
    border: none;
    background-color: #fff;
}

</style>