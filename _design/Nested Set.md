# Nested Set

## Concept

[Basic](https://en.wikipedia.org/wiki/Nested_set_model)

## Design

Set('name[Node]', 'lft[Left]', 'rgt[Right]', 'level[Depth]', ['Parent Key'])

## Operate

### insert

    SELECT @parentLeft:=lft FROM Tree WHERE id='parent key'; -- root node already exists
    UPDATE Tree SET rgt=rgt+2 WHERE rgt>@parentLeft; -- update right edge after parent left
    UPDATE Tree SET lft=lft+2 WHERE lft>@parentLeft; -- update left edge after parent left
    INSERT INTO Tree(id,lft,rgt) VALUES ('node key', @parentLeft+1, @parentLeft+2); -- insert node

### delete

    SELECT @myLeft:=lft,@myRight:=rgt,@mySpan:=rgt-lft+1 FROM Tree WHERE id='node key'; -- root can not be deleted
    DELETE FROM Tree WHERE lft BETWEEN @myLeft AND @myRight; -- delete children node
    UPDATE Tree SET rgt=rgt-@mySpan WHERE rgt>@myLeft; -- update right edge after current node
    UPDATE Tree SET lft=lft-@mySpan WHERE lft>@myLeft; -- update left edge after current node

    -- No children or delete recursive
    SELECT @myLeft:=lft,@myRight:=rgt FROM Tree WHERE id='node key'; -- root can not be deleted
    UPDATE Tree SET rgt=rgt-2 WHERE rgt>@myLeft;
    UPDATE Tree SET lft=lft-2 WHERE lft>@myLeft;

### find
    
    SELECT node.name
    FROM Tree as node left join Tree as parent
    WHERE node.lft BETWEEN parent.lft AND parent.rgt AND parent.name='parent_name'
    ORDER BY node.lft

## Category and Tag

Category [1..n] Content_Category [n..1] Content

Tag [1..n] Content_Tag [n..1] Content