## CSS in Styled Components

```
const iconStyles = css`
  width: 16px;
  height: 16px;
  cursor: pointer;
`;

const StyledIcons = styled(FilledFilter)`
  ${iconStyles};
  color: ${({ theme }) => theme.palette.colors.green};
`;
```

___

## makeStyles usage

```
// styles.ts
import { makeStyles } from '@material-ui/core'

export const useStyles = makeStyles({
  title: {
    minWidth: 144,
  },
})

// component
import { useStyles } from './styles'

const Component = () => {
  const classes = useStyles()

  return <div className={classes.title}></div>
}
```

___

## Props in Styled components

```
interface IExtendedProps extends IProps {
  flexGrow: number;
}

const StyledForm = styled(Form)<IExtendedProps>`
  flex: ${({ flexGrow }) => flexGrow} 1 0;
`;
```

___


