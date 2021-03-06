______________________________________________________________________________

### `GenericManagementList.props.attr`

```html
<GenericManagementList attr={...}>
    <div className='list-container' {...props.attr.container}>
        <div className='column' {...props.attr.column}>
            <div className='column-header' {...props.attr.selectAllContainer>
                <div className='checkbox-empty' {...props.attr.selectAllEmpty}/>
                or
                <CheckboxInput attr={props.attr.selectAllCheckbox}/>
            </div>
            <div className='column-content' {...props.attr.selectRowContent}>
                <CheckboxInput attr={props.attr.selectRowCheckbox}/>
            </div>
            ...
            <div className='column-content' {...props.attr.selectRowContent}>
                <CheckboxInput attr={props.attr.selectRowCheckbox}/>
            </div>
        </div>
        <div className='column' {...props.attr.column}>
            <button className='column-header' {...props.attr.rowHeaderButton}>
                Column Title <span className='sort-direction' {...props.attr.rowHeaderChevron}/>
            </button>
            <div className='column-content' {...props.attr.rowContent}>
                Row 1 Column 1
            </div>
            ...
            <div className='column-content' {...props.attr.rowContent}>
                Row N Column 1
            </div>
        </div>
        ...
        <div className='column' {...props.attr.column}>
            <button className='column-header' {...props.attr.rowHeaderButton}>
                Column Title <span className='sort-direction' {...props.attr.rowHeaderChevron}/>
            </button>
            <div className='column-content' {...props.attr.rowContent}>
                Row 1 Column N
            </div>
            ...
            <div className='column-content' {...props.attr.rowContent}>
                Row N Column N
            </div>
        </div>
    </div>
</GenericManagementList>
```

______________________________________________________________________________

### Example

```jsx
class ManagementListDemo extends React.Component {
    constructor() {
        super();

        this.rows = [
            {
                name: 'Row Name1',
                owner: 'Owner Name1',
                lastUpdated: new Date(),
                classification: ['owner'],
                actions: [],
                attr: {
                    'data-test-hook': 'row-1'
                }
            },
            {
                name: 'Row Name2',
                owner: 'Owner Name2',
                lastUpdated: new Date(),
                classification: ['owner', 'test'],
                actions: []
            },
            {
                name: 'Row Name3',
                owner: 'Owner Name3',
                lastUpdated: new Date(),
                classification: ['not-owner', 'not-test'],
                actions: []
            },
            {
                name: 'Row Name4',
                owner: 'Owner Name4',
                lastUpdated: new Date(),
                classification: ['not-owner'],
                actions: []
            }
        ];

        const onAscending = (index) => () => this.setState({
            sortedColumn: this.cols[index],
            sortDirection: 'ascending'
        });
        const onDescending = (index) => () => this.setState({
            sortedColumn: this.cols[index],
            sortDirection: 'descending'
        });

        this.cols = [
            {
                label: 'NAME',
                mapColumn: (row) => row.name,
                onAscending: onAscending(0),
                onDescending: onDescending(0),
                hidden: false,
                width: 100
            },
            {
                label: 'OWNER',
                mapColumn: (row) => row.owner,
                onAscending: onAscending(1),
                onDescending: onDescending(1),
                hidden: false,
                width: 100
            },
            {
                label: 'LAST UPDATED',
                mapColumn: (row) => row.lastUpdated.toUTCString(),
                onAscending: onAscending(2),
                onDescending: onDescending(2),
                hidden: false,
                width: 200
            },
            {
                label: 'CLASSIFICATION',
                mapColumn: (row) => row.classification.join(' - '),
                hidden: false,
            }
        ];

        const selected = this.rows.map(row => false);

        this.state = {
            sortedColumn: null,
            sortDirection: null,
            selected: selected,
        };
    }

    onSelect(row, newValue) {
        const selected = [...this.state.selected];
        const index = this.rows.indexOf(row);
        if (index > -1) {
            selected[index] = newValue;
            this.setState({selected: selected});
        }
    }

    onSelectAll(newValue) {
        const selected = this.rows.map(row => newValue);
        this.setState({
            selected: selected
        });
    }

    isSelected(row) {
        const index = this.rows.indexOf(row);
        if (index > -1) {
            return this.state.selected[index];
        }
        return false;
    }

    onSort(column, direction) {
        this.setState({
            sortedColumn: column,
            sortDirection: direction
        });
    }

    render() {
        let rows;
        if (this.state.sortedColumn) {
            const col = this.state.sortedColumn;
            if (this.state.sortDirection === 'descending') {
                rows = this.rows.sort((first, second) =>
                    (col.mapColumn(first) > col.mapColumn(second)
                        ? -1
                        : (col.mapColumn(second) > col.mapColumn(first) ? 1 : 0)
                    )
                );
            } else {
                rows = this.rows.sort((first, second) =>
                    (col.mapColumn(first) > col.mapColumn(second)
                        ? 1
                        : (col.mapColumn(second) > col.mapColumn(first) ? -1 : 0)
                    )
                );
            }
        } else {
            rows = this.rows;
        }
        return <GenericManagementList
            rows={rows}
            columns={this.cols}

            onSelect={this.onSelect.bind(this)}
            onSelectAll={this.onSelectAll.bind(this)}
            isSelected={this.isSelected.bind(this)}

            selectLabel={row => row.label}

            sortedColumn={this.state.sortedColumn}
            sortDirection={this.state.sortDirection}
        />
    }
}

<div style={{padding: '8px'}}>
    <ManagementListDemo />
</div>
```
