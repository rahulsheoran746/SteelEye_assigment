# Explain what the simple List component does.
>>
List component take arrays of items as props and generates an unordered list of items.
where each items are itself a component which has text of the list item, index. it also keep 
record where the is selected or not using useState hook and lastly a reference of function is
passed to child component singlelistItem where on click of that list item handleClick function
of List component get executed. this function take index as parameter and then set the 
setselectedIndexto that index to change its color from red to green.

# What problems / warnings are there with code?
>>
The problems/ warnings in the code are -:
1.	in list component, useState takes inital value which is not given 
2.	items as depedency in the useEffect is not required.
3.	using map to create multiple component requires a key params to uniquely identify that
	child component
4.	logic for isSelected params is wrong according to the code it stores the index not a boolean
5. 	In List item component onClick params is executing the function parsing code only,where
	as it	should just point to that function and execute when clicked

# code

    import React, { useState, useEffect, memo } from 'react';
    
    // Single List Item
    const WrappedSingleListItem = ({
      index,
      isSelected,
      onClickHandler,
      text,
    }) => {
      const clickhandler=()=>{
        onClickHandler(index)
      }
      return (
        <li
          style={{ backgroundColor: isSelected ? 'green' : 'red'}}
          onClick={clickhandler}
        >
          {text}
        </li>
      );
    };
    
    WrappedSingleListItem.propTypes = {
      index: PropTypes.number,
      isSelected: PropTypes.bool,
      onClickHandler: PropTypes.func.isRequired,
      text: PropTypes.string.isRequired,
    };
    
    const SingleListItem = memo(WrappedSingleListItem);
    
    // List Component
    const WrappedListComponent = ({
      items,
    }) => {
      const [selectedIndex,setSelectedIndex] = useState(null);
    
      useEffect(() => {
        setSelectedIndex(null);
      }, []);
    
      const handleClick = index => {
        console.log(index)
        setSelectedIndex(index);
      };
    
      return (
        <ul style={{ textAlign: 'left' }}>
          {items.map((item, index) => (
            <SingleListItem
              onClickHandler={handleClick}
              key={index}
              text={item.text}
              index={index}
              isSelected={selectedIndex==index ? true:false}
            />
          ))}
        </ul>
      )
    };
    
    WrappedListComponent.propTypes = {
      items: PropTypes.array(PropTypes.shapeOf({
        text: PropTypes.string.isRequired,
      })),
    };
    
    WrappedListComponent.defaultProps = {
      items: null,
    };
    
    
    const List = memo(WrappedListComponent);
    
    export default List;
