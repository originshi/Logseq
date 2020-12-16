---
title: imis内部组件使用
---

## LATER crud
```

{
    defaultFormName:'smallClassForm',
	type: 'crud',  //类型
	watchName: ['smallClassRefresh'],  //
    //const {store} = useStore(parentId, 'smallClassRefresh');
    //store.getObservable('formSubmit').set({type: 'reload'});
	"api":  {
		url: "/fairy/subclazz/parentsMetting/list",
		requestAdaptor: `
			var {
				status,
				sortType
			} = data;
			if (sortType === 1) {
				data.sortType = 'ascend';
			} else if (sortType === 2) {
                data.sortType = 'descend';
			} 
			data.status = status ? +status : null;
			return data;
		`,
		responseAdaptor: `
			var {
				data: list,
				pager: {
                    total,
					current,
					pageSize
				},
                code,
                msg
			} = data;
			return {
				data: {
					list,
				},
				pager: {
                    total,
					current,
					pageSize
				},
                code,
                msg
			};
		`,
	},
	search: {
        //搜索条件
		controls: [
			{
				type: 'radioButton',
				key: 'status',
				initValue: 0,
				method: 'post',
				source: {
					api: '/fairy/subclazz/parentsMetting/count',
					data: {1: '{radioBtnRefresh}',},
					requestAdaptor: `
						var d = {...data};
						delete d[1];
						return d
					`,
					responseAdaptor: `
						var list = data.data;
						return {data: {list: list.map((item) => {
							return {
							label: item.label + "(" + item.count + ")",
							key: item.status
							}
						})}}
					`,
					method: 'post'
				},
				submitOnChange: true
			},
		],
       / 
		],
		useAction: false
	},
	actions: [
		{
			type: 'newSmallClass',
		}
	],
	body: {
		type: 'table',
		options: {
			defaultPageSize: 50
		},
		columns: [
			{
				title: '小班课名称',
				key: 'lessonName',
				dataIndex: 'lessonName',
				"type": "text",
			},
			{
				title: '小班课开始-结束时间',
				key: 'lessonBeginTime',
				dataIndex: 'lessonBeginTime',
				sorter: true,
				type: 'format',
				format: {
					data: [{
						strFunction: 'return record',
						key: 'record'
					}],
					source: [{key: 'record', type: 'context'}],
					body: {
						type: 'fairyDataRange'
					}
				}
			},
			{
				title: '小班课时长',
				key: 'id',
				dataIndex: 'id',
				"type": "text",
				type: 'format',
				format: {
					data: [{
						strFunction: 'return record',
						key: 'record'
					}],
					source: [{key: 'record', type: 'context'}],
					body: {
						type: 'fairyLength'
					}
				}
			},
			{
				title: '小班课参与人数',
				key: 'statusText',
				dataIndex: 'statusText',
				type: 'format',
				format: {
					data: [{
						strFunction: 'return record',
						key: 'record'
					}],
					source: [{key: 'record', type: 'context'}],
					body: {
						type: 'smallClassStudentList'
					}
				}
			},
			{
				title: '小班课状态',
				key: 'status',
				dataIndex: 'status',
				type: 'format',
				format: {
					data: [{
						strFunction: 'return record',
						key: 'record'
					}],
					source: [{key: 'record', type: 'context'}],
					body: {
						type: 'smallClassStatus'
					}
				}
			},
			{
				title: '操作',
				key: 'id',
				dataIndex: 'id',
				type: 'format',
				format: {
					data: [{
						strFunction: 'return record',
						key: 'record'
					}],
					source: [{key: 'record', type: 'context'}],
					body: {
						type: 'smallClassOperation'
					}
				}
			},
		]
	}
}
```