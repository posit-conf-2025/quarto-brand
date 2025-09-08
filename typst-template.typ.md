#let whr-template(
  doc,
  title: none,
  author: none,
  cover_image: none,
  main_logo: none,
  bottom_logos: none,
  toc_background_image: none,
) = {
  // Page setup for the entire document
  set text(font: "Linux Libertine", size: 10pt)
  set heading(numbering: "1.1")
  set page(paper: "a4", margin: (left: 20mm, right: 30mm, top: 20mm, bottom: 20mm))

  // Define the style for purple headings
  show heading: it => {
    set text(fill: rgb("#4B0082")) // A deep purple
    it
  }

  // Define the blue bar on the right side of pages
  #show page: page => {
    // Add the blue bar (except on the cover page)
    if counter(page).at(1) > 1 {
      box(
        width: 10pt,
        height: 100%,
        fill: rgb("#2a87d6"),
        float: true,
        inset: 0pt,
        place(right, top)
      )
    }
    // Render the page content
    page
  }

  // Cover page layout
  page(
    header: none,
    footer: none,
    background: cover_image,
  )[
    #place(
      right, center,
      image(main_logo)
    )

    #if bottom_logos != none {
      #place(bottom, center, [
        #h(1fr) #bottom_logos #h(1fr)
      ])
    }
  ]

  // Table of Contents page
  page(
    header: none,
    footer: none,
    background: toc_background_image,
  )[
    #show: heading.where(level: 1, 2)
    #outline(title: "Contents")
  ]

  // Main content pages
  #set page(columns: 2)
  #set page(
    header: if title != none {
      box(width: 100%, [
        #h(1fr) #text(9pt)[#title]
      ])
    },
    footer: box(
      width: 100%,
      [#h(1fr) #text(9pt)[#counter(page).at(1)]]
    )
  )

  // Insert the main document content
  #doc
}

// Apply the template to the document with the required parameters
#show: doc => whr-template(
  doc,
  title: [WORLD HAPPINESS REPORT 2024],
  cover_image: "../assets/cover-page.jpg",
  main_logo: "../assets/logo-center-right.png",
  bottom_logos: [
    #image("../assets/bottom-logo-1.png", height: 1.5cm)
    #h(1cm)
    #image("../assets/bottom-logo-2.png", height: 1.5cm)
    #h(1cm)
    #image("../assets/bottom-logo-3.png", height: 1.5cm)
  ],
  toc_background_image: "../assets/toc-background.jpg",
)