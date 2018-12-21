<style scoped>
    .expand-row{
        margin-bottom: 16px;
    }
</style>
<template>
    <div>
        <Row class="expand-row">
            <Col span="8">
                <span class="expand-key">参与人类型: </span>
                <span class="expand-value">{{ row.travelNoteStyle }}</span>
            </Col>
            <Col span="8">
                <span class="expand-key">游记浏览量: </span>
                <span class="expand-value">{{ row.travelNoteBrowses }}</span>
            </Col>
            <Col span="8">
                <span class="expand-key">游记评论量: </span>
                <span class="expand-value">{{ row.travelNoteDiscuss }}</span>
            </Col>
        </Row>
        <Row>
            <Col span="8">
                <span class="expand-key">游记喜欢量: </span>
                <span class="expand-value">{{ row.travelNoteLikes }}</span>
            </Col>
            <Col span="8">
                <span class="expand-key">游记收藏量: </span>
                <span class="expand-value">{{ row.travelNoteCollects }}</span>
            </Col>
            <Col span="8">
                <span class="expand-key">游记分享量: </span>
                <span class="expand-value">{{ row.travelNoteShares }}</span>
            </Col>
            <Col span="8">
                <span class="expand-key">旅游的目的地: </span>
                <span class="expand-value">{{ row.travelNoteDestination }}</span>
            </Col>
        </Row>
    </div>
</template>
<script>
export default {
  props: {
    row: Object
  }
}
</script>
