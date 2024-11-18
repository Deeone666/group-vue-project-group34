<template>
    <div>
        <h1>Medal Table</h1>
        <el-button type="primary" @click="showAddModal = true" style="margin-bottom: 20px;">Add New Item</el-button>
        <el-table :data="medalists" style="width: 100%" border>
            <el-table-column prop="nationality" label="Nationality"></el-table-column>
            <el-table-column prop="name" label="Name"></el-table-column>
            <el-table-column>
                <template slot="header">
                    Gold Medal
                    <img src="@/assets/gold-medal.png" alt="Gold Medal"
                        style="width: 20px; margin-left: 5px; margin-bottom: -5px;" />
                </template>
                <template slot-scope="scope">
                    {{ scope.row.gold }}
                    <img src="@/assets/gold-medal.png" alt="Gold Medal"
                        style="width: 20px; margin-left: 5px; margin-bottom: -5px;" />
                </template>
            </el-table-column>

            <el-table-column>
                <template slot="header">
                    Silver Medal
                    <img src="@/assets/silver-medal.png" alt="Silver Medal"
                        style="width: 20px; margin-left: 5px; margin-bottom: -5px;" />
                </template>
                <template slot-scope="scope">
                    {{ scope.row.silver }}
                    <img src="@/assets/silver-medal.png" alt="Silver Medal"
                        style="width: 20px; margin-left: 5px; margin-bottom: -5px;" />
                </template>
            </el-table-column>

            <el-table-column>
                <template slot="header">
                    Bronze Medal
                    <img src="@/assets/bronze-medal.png" alt="Bronze Medal"
                        style="width: 20px; margin-left: 5px; margin-bottom: -5px;" />
                </template>
                <template slot-scope="scope">
                    {{ scope.row.bronze }}
                    <img src="@/assets/bronze-medal.png" alt="Bronze Medal"
                        style="width: 20px; margin-left: 5px; margin-bottom: -5px;" />
                </template>
            </el-table-column>

            <el-table-column label="Total Medals">
                <template slot-scope="scope">{{ getTotalMedals(scope.row) }}</template>
            </el-table-column>
            <el-table-column label="Actions">
                <template slot-scope="scope">
                    <el-button type="danger" @click="deleteMedalist(scope.$index)">Delete</el-button>
                </template>
            </el-table-column>
        </el-table>

        <el-dialog title="Add a New Medalist" :visible.sync="showAddModal">
            <el-form :model="newMedalist">
                <el-form-item label="Nationality">
                    <el-input v-model="newMedalist.nationality" placeholder="Enter Nationality"></el-input>
                </el-form-item>
                <el-form-item label="Name">
                    <el-input v-model="newMedalist.name" placeholder="Enter Name"></el-input>
                </el-form-item>
                <el-form-item label="Gold Medal">
                    <el-input-number v-model="newMedalist.gold" :min="0"></el-input-number>
                </el-form-item>
                <el-form-item label="Silver Medal">
                    <el-input-number v-model="newMedalist.silver" :min="0"></el-input-number>
                </el-form-item>
                <el-form-item label="Bronze Medal">
                    <el-input-number v-model="newMedalist.bronze" :min="0"></el-input-number>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="showAddModal = false">Cancel</el-button>
                <el-button type="primary" @click="addMedalist">Create</el-button>
            </div>
        </el-dialog>
    </div>
</template>

<script>
export default {
    data() {
        return {
            // 默认数据
            defaultMedalists: [
                { nationality: 'CHN', name: 'ZHANG Fei', gold: 5, silver: 1, bronze: 5 },
                { nationality: 'USA', name: 'ROBERT Goldman', gold: 3, silver: 2, bronze: 1 },
                { nationality: 'CAN', name: 'ALBERT Summer', gold: 2, silver: 1, bronze: 2 },
                { nationality: 'NZL', name: 'JACK Watson', gold: 0, silver: 1, bronze: 1 },
            ],
            medalists: [], // 奖牌列表数据
            newMedalist: {
                nationality: '',
                name: '',
                gold: 0,
                silver: 0,
                bronze: 0,
            },
            showAddModal: false,
        };
    },
    created() {
        this.loadMedalists();
    },
    methods: {
        getTotalMedals(medalist) {
            return medalist.gold + medalist.silver + medalist.bronze;
        },
        addMedalist() {
            if (this.newMedalist.nationality && this.newMedalist.name) {
                this.medalists.push({ ...this.newMedalist });
                this.saveMedalists();
                this.resetNewMedalist();
                this.showAddModal = false;
            } else {
                this.$message.error('Please fill in all fields.');
            }
        },
        deleteMedalist(index) {
            this.medalists.splice(index, 1);
            this.saveMedalists();
        },
        resetNewMedalist() {
            this.newMedalist = {
                nationality: '',
                name: '',
                gold: 0,
                silver: 0,
                bronze: 0,
            };
        },
        saveMedalists() {
            localStorage.setItem('medalists', JSON.stringify(this.medalists));
        },
        loadMedalists() {
            const savedData = localStorage.getItem('medalists');
            if (savedData) {
                this.medalists = JSON.parse(savedData);
            } else {
                // 如果本地没有数据，则加载默认数据并保存到 localStorage
                this.medalists = [...this.defaultMedalists];
                this.saveMedalists();
            }
        },
    },
};
</script>

<style scoped>
.dialog-footer {
    text-align: right;
}
</style>
