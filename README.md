
1.Explain what the simple List component does.

The React List component displays a collection of items passed through the "items" prop. Each item is represented by the SingleListItem component, which accepts "isSelected", "index", "onClickHandler", and "text" props. When an item is clicked, the "handleClick" function is triggered, updating the "selectedIndex" state to the index of the clicked item. The "isSelected" prop is then passed to the SingleListItem component to determine if the item is selected and display a green or red background color accordingly.


2.What problems / warnings are there with code?

In the SingleListItem component, the onClickHandler prop should be passed as a function reference, rather than invoking it immediately. The line should be changed from:
onClick={onClickHandler(index)}         to      onClick={() => onClickHandler(index)}

In the WrappedListComponent component, the selectedIndex state hook is not initialized.these are the changes:
const [setSelectedIndex, selectedIndex] = useState();     to    const [selectedIndex, setSelectedIndex] = useState(null);

In the WrappedListComponent component, the items prop should be defined as an array of objects with a text property, rather than an array with a shape of an object. The line should be changed from:
items: PropTypes.array(PropTypes.shapeOf({
text: PropTypes.string.isRequired,
})), to
items: PropTypes.arrayOf(PropTypes.shape({
text: PropTypes.string.isRequired,
})),

In the SingleListItem component, the isSelected prop which is a boolean. It should be passed as a comparison between index and selectedIndex:
isSelected={index === selectedIndex}




3. import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

const SingleListItem = memo(({index,isSelected,onClickHandler,text}) => {
  return (
    <li  style={{ backgroundColor: isSelected ? 'green' : 'red' }}
        onClick={() => onClickHandler(index)>
      {text}
    </li>
  );
});

SingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const List = memo(({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState(null);

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={handleClick}
          text={item.text}
          index={index}
          isSelected={index === selectedIndex}
        />
      ))}
    </ul>
  );
});

List.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ).isRequired,
};
List.defaultProps = { 
    items: [
      { text: "ABC" },
      { text: "DEF" },
      { text: "GHI" },
      { text: "JKL" },
      { text: "MNO" },
  
    ],
  };

export default List;
