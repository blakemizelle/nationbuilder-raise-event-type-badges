# NationBuilder Page Tags to Event Type Badges (Raise Theme Example)

A proof-of-concept implementation showing how to transform NationBuilder page tags into styled event type badges, built as a partial for the Raise theme. This example demonstrates how to create a maintainable tag-based styling system using Bootstrap and Liquid templating.

## Important Notes

- This is an example implementation for the NationBuilder Raise theme
- Requires Bootstrap 5 classes (included in Raise theme)
- May need modification for other themes or styling frameworks
- Intended as a learning resource/starting point, not a drop-in solution

## Context

This code lives in `_event_card.html` partial in the Raise theme and handles the display of event type badges on event cards. While specific to this context, the approach demonstrates useful patterns for:
- Managing theme configurations without code
- Transforming tags into styled elements
- Creating maintainable admin interfaces

## The Problem It Solves

Many NationBuilder administrators need to:
1. Categorize events visually
2. Allow non-technical users to manage event types
3. Maintain consistent styling across event displays
4. Avoid touching code for routine updates

The traditional approach often involves:
- Custom CSS classes for each event type
- Hard-coded conditional statements
- Direct HTML editing for changes
- Technical knowledge requirements

## The Solution

This implementation creates a configuration-driven system where:
1. Event types are defined in a simple configuration block
2. Administrators use standard NationBuilder tags
3. Styling automatically applies based on configuration
4. No code editing is required for routine changes

### Configuration Example

```liquid
{% capture event_types %}
Live Online Programme => bg-warning|text-dark
Local Activism => bg-success|text-white
Community Event => bg-primary|text-white
Training Session => bg-info|text-white
Campaign Action => bg-danger|text-white
{% endcapture %}
```

### Usage for Administrators

1. Add tags to events in format: `Event type | Your Display Name`
2. The system automatically applies the configured styling
3. To add new types, just add to the configuration block

## Technical Implementation

The solution uses Liquid's template processing to create a pseudo hash map:

```liquid
{% for tag in page.tags %}
  {% if tag_name contains "Event type |" %}
    {% assign event_type = tag_name | remove: "Event type |" | strip %}
    {% assign event_types_array = event_types | split: "
" %}
    
    {% for type_def in event_types_array %}
      {% assign type_parts = type_def | split: "=>" %}
      {% if type_parts[0] | strip == event_type %}
        {% assign type_colors = type_parts[1] | strip | split: "|" %}
        <span class="event-type-tag badge {{ type_colors[0] }} {{ type_colors[1] }}">
          {{ event_type }}
        </span>
      {% endif %}
    {% endfor %}
  {% endif %}
{% endfor %}
```

## Available Styling (Bootstrap 5)

**Background Colors:**
- `bg-warning` (yellow)
- `bg-success` (green)
- `bg-primary` (blue)
- `bg-info` (light blue)
- `bg-danger` (red)

**Text Colors:**
- `text-white`
- `text-dark`

## Adapting for Other Themes

To adapt this approach for other themes:

1. Ensure Bootstrap 5 is available or replace classes with your framework
2. Modify the HTML structure to match your theme's patterns
3. Adjust the configuration syntax if needed
4. Test thoroughly with your theme's styling

## Debug Mode

The implementation includes console logging to help troubleshoot:
```liquid
<script>
console.group('Event Type Debug');
console.log('Raw event_types: {% raw %}{{ event_types }}{% endraw %}');
console.log('Processing tag: {% raw %}{{ tag_name }}{% endraw %}');
console.log('Found event type: {% raw %}{{ event_type }}{% endraw %}');
console.log('Event types array: {% raw %}{{ event_types_array | join: "," }}{% endraw %}');
console.log('Type parts: {% raw %}{{ type_parts | join: "," }}{% endraw %}');
console.groupEnd();
</script>
```

## Contributing

This is a proof-of-concept implementation. Feel free to:
- Suggest improvements
- Share adaptations for other themes
- Propose alternative approaches
- Report issues or limitations

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- Built for NationBuilder CMS
- Uses Bootstrap 5 utility classes
- Inspired by real-world admin needs
- Based on the Raise theme structure

---

*Built with ❤ for the NationBuilder community*
