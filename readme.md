# `indemmar` - context managers for `matplotlib` plots

The module allows a more DRY approach to using the `matplotlib` interface.
For example, instead of:

```python
fig, ax = plt.subplots()
ax.plot([1, 3, 1, 4, 5, 1, 0])
fig.tight_layout()
fig.savefig('fig/simple.png')
```

We would write:

```python
with plot(fname='fig/simple.png') as (fig, ax):
	ax.plot([1, 3, 1, 4, 5, 1, 0])
```

Additionally, the module wraps a sane approach to dealing with plot legends by creating a new
subplot for the legend, which means that the legend won't be cliped or overlap the plot itself:

```python
with plot_and_legend as (fig, ax):
	ax.plot([1, 3, 1, 4, 5, 1, 0], label='Data')
```

The context manager will pick up the labels used for creating the figure. This sacrifices the
customizability somewhat, since the user doesn't have access to the legend object. A more explicit
approach would be to create a legend subplot layout with `adaptable_legend_subplot`, and then
customize the legend:

```python
fig, lax, ax = adaptable_legend_subplot(nrow, ncol, legend_side, **figure_kwargs)

h, l = get_all_handles_and_labels(fig)
lax.legend(h, l, bbox_transform=lax.transAxes, bbox_to_anchor=[0.5, 0.5], loc='center')
```

The module is necessarilly opinionated, but makes most of its behaviour customizable.
