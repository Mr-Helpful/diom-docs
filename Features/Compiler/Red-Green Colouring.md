Colour the query tree using update token spans to decide what results to update.
Then perform the updates from the top.

This is probably going to be implemented with an attribute macro to build up the tree with every pass, going from:

```rs
#[tree_track] // or some better name
fn parse_type(input: &str) -> Result<(&str, Type), Error> {
  todo!("function body")
}
```

to something like:

```rs
fn parse_type(tree: &mut QueryTracker, input: &str) -> Result<(&str, Type), Error> {
  tree.open_node("parse_type");
  let result = if tree.is_red() {
    todo!("function body")
  } else {
    parse_type_cache.get(input).unwrap()
  };
  tree.close_node("parse_type");
  result
}
```

where we can use the token spans that nodes operated over to mark them as red (i.e. recalculation needed).